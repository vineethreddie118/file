public handleLocationExpirationDate(newLocationExpirationDate: any, nIndex: number, location: any) {
    console.log("Date provided = ", newLocationExpirationDate);

    // Convert the date to MM/dd/yyyy format
    const dateObj = new Date(newLocationExpirationDate);
    const formattedExpirationDate = 
        ('0' + (dateObj.getMonth() + 1)).slice(-2) + '/' + 
        ('0' + dateObj.getDate()).slice(-2) + '/' + 
        dateObj.getFullYear();

    if (!location) { // For Provider Location
        let records = this.oProviderLocation.providerLocationInfo[nIndex].providerLocBillingInfo;
        records = records.map(record => {
            record.expirationDate = formattedExpirationDate;  // Set formatted date
            return record;
        });
        this.oProviderLocation.providerLocationInfo[nIndex].providerLocBillingInfo = records;
        this.oProviderLocation.providerLocationInfo[nIndex].isDelete = true;
        return nIndex;
    } else { // For Billing Location
        let currIndex = 0;
        let result;
        this.oProviderLocation.providerLocationInfo.map((locationInfo, i) => {
            const len = locationInfo.providerLocBillingInfo ? locationInfo.providerLocBillingInfo.length : 0;
            if (len > nIndex - currIndex) {
                if (location.providerSvcLocPayeeId === locationInfo.providerLocBillingInfo[nIndex - currIndex].providerSvcLocPayeeId) {
                    locationInfo.providerLocBillingInfo[nIndex - currIndex].expirationDate = formattedExpirationDate; // Set formatted date
                    if (this.replaceBillingExpirationDate) {
                        locationInfo.providerLocBillingInfo[nIndex - currIndex].isReplace = true;
                    } else {
                        locationInfo.providerLocBillingInfo[nIndex - currIndex].isDelete = true;
                    }
                    result = i;
                }
            } else {
                currIndex += len;
            }
        });
        return result;
    }
}
