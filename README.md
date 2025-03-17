 public handleLocationExpirationDate(newLocationExpirationDate: any, nIndex: number, location: any) {
    console.log("Date provided = ", newLocationExpirationDate);
    const newExpirationDate = new Date(newLocationExpirationDate).toISOString().split('.')[0];
    if (!location) { // For Provider Location
      let records = this.oProviderLocation.providerLocationInfo[nIndex].providerLocBillingInfo;
      records = records.map(record => {
        record.expirationDate = newExpirationDate;
        return record;
      });
      this.oProviderLocation.providerLocationInfo[nIndex].providerLocBillingInfo = records;
      this.oProviderLocation.providerLocationInfo[nIndex].isDelete = true;
      return nIndex;
    } else { // For Billing Location
      let currIndex = 0;
      let result;
      this.oProviderLocation.providerLocationInfo.map((locationInfo, i) => {
        const len = locationInfo.providerLocBillingInfo? locationInfo.providerLocBillingInfo.length : 0;
        if (len > nIndex - currIndex) {
          if (location.providerSvcLocPayeeId === locationInfo.providerLocBillingInfo[nIndex - currIndex].providerSvcLocPayeeId) {
           locationInfo.providerLocBillingInfo[nIndex - currIndex].expirationDate = newExpirationDate;
           if(this.replaceBillingExpirationDate){
             locationInfo.providerLocBillingInfo[nIndex - currIndex].isReplace = true;
            }else{
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

 public fnTerminateLocation(nIndex: number, modal: any, location: any = null,isBillingAddress): void {
    if(this.isBillingEditMode || this.isEditMode || this.isReplaceMode || this.isReplaceBillingMode||this.serviceLocations && this.serviceLocations.length>0){
      this.addService.fnRaiseWarningDlg("Invalid","Edit changes should be saved or discarded");
      return;
    }
    if (true === location.isGrpServiceLocation && location?.groupInfo && this.nProviderId !=location?.groupInfo[0].providerId  ) {
      this.addService.fnRaiseWarningDlg("Not allowed","This Tax ID belongs to an organization. You need to edit this address in the organization rather from here.");
      return;
    }
    if(isBillingAddress && this.hasSingleBillingInfo(location.serviceLocationId)){
      this.addService.fnRaiseWarningDlg("Not allowed","Provider must have at least one active billing location. If you are attempting to terminate this billing location, please add a new active billing location for this site prior to this action.");
      return;
    }

    location = Object.keys(location).includes('billingAddressId')?location:null;

    this.isPracticeLocationAccordionOpen = false;
    this.isReplaceBillingMode = false;
    this.isTerminateBtnClicked = false;
    this.isBillingNotProvider = location ? true : false;
    this.sLocationExpirationDate = (location && location.expirationDate) ? location.expirationDate.split('T')[0] : null;
    if (location == null && nIndex >=0) {
      let getExpirationDate = this.oProviderLocation.providerLocationInfo[nIndex].providerLocBillingInfo
                    .filter(s => s.expirationDate !== null && s.expirationDate !== '9999-12-31T00:00:00')
                    .sort();
      const lenOfGetExpirationDate = getExpirationDate.length;
      if ( lenOfGetExpirationDate> 0) {
        this.sLocationExpirationDate = getExpirationDate[lenOfGetExpirationDate-1].expirationDate.split('T')[0];
      }
    }

    // Creating a function to open the modal to input new expiration date
    const openModal = () => {
      this.oBsModalSvc.open(modal).result.then((closeResult) => {
        if (closeResult === 'terminate') {
          this.isTerminateBtnClicked = true;
          if (!this.sLocationExpirationDate) {
            openModal(); 
            return;
          }
          nIndex = this.handleLocationExpirationDate(this.sLocationExpirationDate, nIndex, location);
          

          const locationRequest = {
            "providerId": this.nProviderId,
            "providerLocationInfo": [this.oProviderLocation.providerLocationInfo[nIndex]]
          }

          this.submitEditedProviderLocationInfo(locationRequest);
          
        }
      }, (reason) => { }
      )
    };
    openModal();

  }
