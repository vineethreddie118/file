 public fnEditLocation(nIndex: number): void {
    if(this.isBillingEditMode || this.isEditMode || this.isReplaceMode || this.isReplaceBillingMode||this.serviceLocations && this.serviceLocations.length>0){
      this.addService.fnRaiseWarningDlg("Invalid","Edit changes should be saved or discarded");
      return;
    }
  
    this.oCurrentProviderLocationInfo = JSON.parse(JSON.stringify({ ...this.oProviderLocation.providerLocationInfo[nIndex] }));

    // this.oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[0].effectiveDate = new Date(this.oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[0].effectiveDate)

    this.isReplaceBillingMode = false;

    this.isPracticeLocationAccordionOpen = true;

    const allDays = ["MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN"]; // Define all possible days
    this.oCurrentProviderLocationInfo.providerSvcLocHours = this.oCurrentProviderLocationInfo.providerSvcLocHours || []; // Initialize providerSvcLocHours if it's not an array

    const existingDays = this.oCurrentProviderLocationInfo.providerSvcLocHours.map(hour => hour.dayOfWeek); // Find out which days are missing
    const missingDays = allDays.filter(day => !existingDays.includes(day));

    // Add default hours for missing days
    missingDays.forEach(missingDay => {
        this.oCurrentProviderLocationInfo.providerSvcLocHours.push(this.getDefaultHour(missingDay));
    });

    // Sort providerSvcLocHours by day of the week
    this.oCurrentProviderLocationInfo.providerSvcLocHours.sort((a, b) => {
        return allDays.indexOf(a.dayOfWeek) - allDays.indexOf(b.dayOfWeek);
    });

    // Convert existing times to 12-hour format with AM/PM for display
    this.oCurrentProviderLocationInfo.providerSvcLocHours.forEach(hour => {
        if (hour.openTimeValue) {
            hour.openTimeValue = this.convertTo12HourFormat(hour.openTimeValue);
        }
        if (hour.closeTimeValue) {
            hour.closeTimeValue = this.convertTo12HourFormat(hour.closeTimeValue);
        }
    });

    // Hide the add symbol for all entries initially
    this.oCurrentProviderLocationInfo.providerSvcLocHours.forEach(svcHour => svcHour.showAddSymbol = false);

    // Track the first entry of each day
    
    this.oCurrentProviderLocationInfo.providerSvcLocHours.sort((a, b) => {
      const dayOrder = ["MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN"];
  
      // Sort by day of the week first
      const dayDifference = dayOrder.indexOf(a.dayOfWeek) - dayOrder.indexOf(b.dayOfWeek);
      if (dayDifference !== 0) return dayDifference;
  
      // If same day, sort by openTimeValue
      const openTimeA = this.convertTo24HourFormat(a.openTimeValue);
      const openTimeB = this.convertTo24HourFormat(b.openTimeValue);
  
      return openTimeA.localeCompare(openTimeB); // Sort by time
    });
  
    let seenDays = {};

    // Set `showAddSymbol` to true only for the first entry of each day
    this.oCurrentProviderLocationInfo.providerSvcLocHours.forEach(svcHour => {
        if (!seenDays[svcHour.dayOfWeek]) {
            svcHour.showAddSymbol = true;     // Show the add symbol
            seenDays[svcHour.dayOfWeek] = true;
        }
    });
    // Filter billing addresses associated with this locatio
    this.associatedBillingAddresses = JSON.parse(JSON.stringify(this.oProviderLocation.providerLocationInfo[nIndex].providerLocBillingInfo));
    

    if (!this.oCurrentProviderLocationInfo.providerAlternateIDSection) {
      this.providerNpi = new ProviderNpi();
    this.providerNpi.providerTaxonomy =  [];
    this.providerNpi.npi = '';
      

      this.providerNpi.serviceLocationId = 0;
      this.providerNpi.providerSvcLocNpiId = 0;
      

      this.oCurrentProviderLocationInfo.providerAlternateIDSection = []
      this.oCurrentProviderLocationInfo.providerAlternateIDSection.push({ providerNpi: [], medicaidId: null, medicare: null });

    }else{
      this.oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi.forEach((npi:any) => {
        if(!npi.providerTaxonomy || npi.providerTaxonomy.length === 0){
          npi.providerTaxonomy=  [];
        }
      });
      

    }

    this.oCurrentProviderLocationInfo.providerAlternateIDSection[0].medicare = {
      "locationAlternateXrefId": null,
      "providerServiceLocationMapId": 0,
      "locationAltId": "",
      "effectiveDate": null,
      "expirationDate": null,
      "userId": 0,
      "state": "string"
    }

    


    // if(!this.oCurrentProviderLocationInfo.billingAddress1Current){
    //     this.oCurrentProviderLocationInfo.billingAddress1Current = {
    //       "street_line":this.oCurrentProviderLocationInfo.billingAddress1,
    //       "secondary": this.oCurrentProviderLocationInfo.billingAddress2,
    //       "city": this.oCurrentProviderLocationInfo.billingCity,
    //       "state": this.oCurrentProviderLocationInfo.billingStateCode,
    //       "zipcode": this.oCurrentProviderLocationInfo.billingZip,
    //   }
   // Trimming logic for serviceAddress
  //  const serviceAddress = this.oCurrentProviderLocationInfo.serviceAddress.substring(0, 25);
  //  this.oCurrentProviderLocationInfo.address2 = this.oCurrentProviderLocationInfo.serviceAddress.length > 25 
  //      ? this.oCurrentProviderLocationInfo.serviceAddress.substring(25)
  //      : this.oCurrentProviderLocationInfo.address2;
  const serviceAddress = this.oCurrentProviderLocationInfo.serviceAddress;

  //  this.oCurrentProviderLocationInfo.serviceAddressCurrent = {
  //      "street_line": this.oCurrentProviderLocationInfo.serviceAddress,
  //      "address2": this.oCurrentProviderLocationInfo.serviceAddress.length > 25 
  //      ? this.oCurrentProviderLocationInfo.serviceAddress.substring(25)
  //      : this.oCurrentProviderLocationInfo.address2,
  //      "city": this.oCurrentProviderLocationInfo.city,
  //      "stateCode": this.oCurrentProviderLocationInfo.stateCode,
  //      "zip": this.oCurrentProviderLocationInfo.zip,
  //  }
  this.oCurrentProviderLocationInfo.serviceAddressCurrent = {
    "street_line": this.oCurrentProviderLocationInfo.serviceAddress,
    "address2": this.oCurrentProviderLocationInfo.address2,
    "city": this.oCurrentProviderLocationInfo.city,
    "stateCode": this.oCurrentProviderLocationInfo.stateCode,
    "zip": this.oCurrentProviderLocationInfo.zip,
}
   this.oCurrentProviderLocationInfo.serviceAddress= serviceAddress;

  
    //  this.taxonamy = {
    //   id:this.oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy[0].id,
    //   taxonomyCode:this.oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy[0].taxonomyCode
    //  }
    //  this.alternateIdActive = this.oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy[0].active;
    //   }
    //   this.isSameAsServiceAddress = this.areAddressesSame(
    //     this.oCurrentProviderLocationInfo.serviceAddress, 
    //     this.oCurrentProviderLocationInfo.billingAddress1
    // );
    this.bIsServiceAddressValidated = this.oCurrentProviderLocationInfo.addressValidated;
    
    //this.oCurrentProviderLocationInfo.addressValidated = this.bIsLocationValidated;
    
    this.nEditIndex = nIndex;
    //this.bIsServiceAddressValidated = true;

    this.isEditMode = true;
    this.bIsDirty = true;
    this.bIsAddclicked = false;
    this.Tin = null;
    this.locationsByTin = [];
    this.isBillingEditMode =false;
   
    

    this.oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi.forEach((npiIndex) => {
      npiIndex.providerTaxonomy.forEach((taxonomy) => {
        if (taxonomy.taxonomyCode.length) {
          this.disabledTaxonomies.push(taxonomy.taxonomyCode);
        }
      });
    });

    setTimeout(() => {
      this.addService.scrollToTarget(this.addPlanElement);
    }, 20);
  }

 if(this.providerForm.phone){
              this.providerForm.phone=this.formatNumbersonfetch(this.providerForm.phone);
            }
            if(this.providerForm.fax){
              this.providerForm.fax=this.formatNumbersonfetch(this.providerForm.fax);
            }

 formatNumbersonfetch(value: string): string {

    if (value.length === 10) {
      value = value.replace(/^(\d{3})(\d{3})(\d{4})$/, '$1-$2-$3');
    }

    return value;
  }
