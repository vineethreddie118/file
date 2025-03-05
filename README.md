 fnupdateLocationDetails() {
    if((this.isEditMode && this.iseditaddmode && !this.isbillingaddressclicked)){
      this.addService.fnRaiseWarningDlg("Missing Billing Details" ," Please click Add button in Billing section ");
      return;
    }
    if ( !this.bIsServiceAddressValidated && !this.bIsInvalidAddressAllowed){
      this.addService.fnRaiseWarningDlg("Invalid" ,"Please enter a valid address or accept to save an invalid address (More information is provided below Validate Address button)");
      return;
    }
    if (!this.oCurrentProviderLocationInfo.phone && this.oCurrentProviderLocationInfo.phoneExt) {
      this.addService.fnRaiseWarningDlg("Missing Phone", "Please enter the phone number.");
      return;
    }
  let timeValueInvalid = false;
  const hasImproperNextDayHours = this.oCurrentProviderLocationInfo.providerSvcLocHours.some(hour => {
      if ((hour.openTimeValue && hour.closeTimeValue === "") || (hour.openTimeValue === '' && hour.closeTimeValue)) {
          timeValueInvalid = true;
      }

      const openTime24 = this.convertTo24HourFormat(hour.openTimeValue);
      const closeTime24 = this.convertTo24HourFormat(hour.closeTimeValue);

      const [openHour, openMinutes] = openTime24.split(':').map(Number);
      const [closeHour, closeMinutes] = closeTime24.split(':').map(Number);

      return openHour > closeHour || (openHour === closeHour && openMinutes >= closeMinutes);
  });

  if (timeValueInvalid) {
      this.addService.fnRaiseWarningDlg("Incomplete Hours", "Please fill both open and close time");
      return;
  }

  if (hasImproperNextDayHours) {
      this.addService.fnRaiseWarningDlg("Invalid Hours", "End time exceeds the allowed period for the day.");
      return;
  }

  const hasOverlappingHours = this.oCurrentProviderLocationInfo.providerSvcLocHours.some((currentHour, index, array) => {
      const currentOpenTime24 = this.convertTo24HourFormat(currentHour.openTimeValue);
      const currentCloseTime24 = this.convertTo24HourFormat(currentHour.closeTimeValue);

      const [currentOpenHour, currentOpenMinutes] = currentOpenTime24.split(':').map(Number);
      const [currentCloseHour, currentCloseMinutes] = currentCloseTime24.split(':').map(Number);

      return array.some((checkHour, checkIndex) => {
          if (index === checkIndex) return false;
          if (currentHour.dayOfWeek !== checkHour.dayOfWeek) return false;

          const checkOpenTime24 = this.convertTo24HourFormat(checkHour.openTimeValue);
          const checkCloseTime24 = this.convertTo24HourFormat(checkHour.closeTimeValue);

          const [checkOpenHour, checkOpenMinutes] = checkOpenTime24.split(':').map(Number);
          const [checkCloseHour, checkCloseMinutes] = checkCloseTime24.split(':').map(Number);

          return (
              (currentOpenHour < checkCloseHour || (currentOpenHour === checkCloseHour && currentOpenMinutes < checkCloseMinutes)) &&
              (currentCloseHour > checkOpenHour || (currentCloseHour === checkOpenHour && currentCloseMinutes > checkOpenMinutes))
          );
      });
  });

  if (hasOverlappingHours) {
      this.addService.fnRaiseWarningDlg("Overlap Detected", "Office hours cannot overlap for the same day.");
      return;
  }
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

  // Convert all time values to 24-hour format without 'AM'/'PM'
  this.oCurrentProviderLocationInfo.providerSvcLocHours.forEach(hour => {
      hour.openTimeValue = this.convertTo24HourFormat(hour.openTimeValue);
      hour.closeTimeValue = this.convertTo24HourFormat(hour.closeTimeValue);
  });
    this.showAccordians = false;

    const keysToDelete = [
      'billingAddress1', 'billingAddress2', 'billingZip', 'billingCity',
      'billingCounty', 'billingStateCode', 'billingDefaultCity',
      'billingAddressValidated', 'taxId', 'taxTypeId', 'payeeName',
      'providerSvcLocPayeeId', 'w9flag', 'effectiveDate', 'expirationDate'
    ];


    const locationRequest = {
      "providerId": this.nProviderId,
      "providerLocationInfo": [this.oCurrentProviderLocationInfo]
    }

    delete locationRequest.providerLocationInfo[0].billingAddress1Current;
    delete locationRequest.providerLocationInfo[0].serviceAddressCurrent;
    //
  
//
locationRequest.providerLocationInfo[0].addressValidated = true;
    locationRequest.providerLocationInfo.forEach(locationInfo => {
      if (locationInfo.providerSvcLocHours) {
        locationInfo.providerSvcLocHours = locationInfo.providerSvcLocHours.filter(
          svcHour => !(svcHour.openTimeValue === svcHour.closeTimeValue)
        );
      }
      keysToDelete.forEach(key => {
        delete locationInfo[key];
      });
    });
    locationRequest.providerLocationInfo[0].providerLocBillingInfo = this.providerLocBillingInfo.filter(billingInfo => !billingInfo.providerSvcLocPayeeId || billingInfo['isLocationByTin']);
    
    locationRequest.providerLocationInfo.forEach(locationInfo => {
      if (locationInfo.providerSvcLocHours) {
        locationInfo.providerSvcLocHours.forEach(svcHour => {
          delete svcHour.day; // Delete the day property
           delete svcHour.showAddSymbol; // Delete the showAddSymbol property
        });
      }
    });
    
    locationRequest.providerLocationInfo[0].providerLocBillingInfo = [...this.associatedBillingAddresses,...locationRequest.providerLocationInfo[0].providerLocBillingInfo];

    locationRequest.providerLocationInfo[0].providerLocBillingInfo.forEach(location => {
      delete location.billingAddress1Current;

    })
  
    locationRequest.providerLocationInfo[0].providerAlternateIDSection[0].providerNpi.forEach(providerNpi => {
      providerNpi.providerTaxonomy = providerNpi.providerTaxonomy.filter(taxObj => taxObj.taxonomyCode);
  });

  // Ensure providerTaxonomy is empty if no taxonomy codes are present
  locationRequest.providerLocationInfo[0].providerAlternateIDSection[0].providerNpi.forEach(providerNpi => {
      if (providerNpi.providerTaxonomy.length === 0) {
          providerNpi.providerTaxonomy = [];
      } else {
          providerNpi.providerTaxonomy = providerNpi.providerTaxonomy.map(taxonomy => ({
              id: this.getTaxonamtByCode(taxonomy),
              taxonomyCode: taxonomy?.taxonomyCode,
              isPrimary: taxonomy.isPrimary ? true : false,
              active: taxonomy?.active ? true : false,
              isDelete: taxonomy.isDelete
          }));
      }
  });

  // Filter out NPI entries where npi is empty
locationRequest.providerLocationInfo[0].providerAlternateIDSection[0].providerNpi = 
locationRequest.providerLocationInfo[0].providerAlternateIDSection[0].providerNpi.filter(providerNpi => providerNpi.npi);

  if (this.addService.eProviderType === ProviderTypeEnum.PRACTITIONER) {
      locationRequest.providerLocationInfo[0].providerAlternateIDSection = null;
  }else {
    const hasNpi = locationRequest.providerLocationInfo[0].providerAlternateIDSection[0].providerNpi.some(npiObj => npiObj.npi);
    if (!hasNpi) {
        locationRequest.providerLocationInfo[0].providerAlternateIDSection = null;
    } else {
        delete locationRequest.providerLocationInfo[0].providerAlternateIDSection[0].medicare;
        delete locationRequest.providerLocationInfo[0].providerAlternateIDSection[0].medicaidId;
    }
  }
  if(locationRequest.providerLocationInfo){
    this.fnSetInactiveLocationSectionVisibility(false);
    this.fnSetInactiveBillingLocationSectionVisibility(false);
    this.bIsLoading = true;
    
    this.addService.saveLocationInformation(locationRequest).subscribe((res: any) => {
      this.oCurrentProviderLocationInfo = new providerLocationInfo();
      this.fnFetchData()
      this.serviceLocations =[]
      
     
      this.bIsDirty = false;
      this.bIsLoading = false;
    },
      err => {
        this.bIsLoading = false;

        this.addService.fnRaiseGenericAlert('savePracticeLocation', err);
        
        this.fnFetchData()
        this.nEditIndex = -1;
        this.isEditMode = false;
        

      })
    }

     

    this.selectedBillingAddressId = undefined;
    this.addNewBillingAddress = false;
    this.showAccordians = false;
    this.isEditMode = false;
    this.bIsDirty = false;
    this.addNewBillingAddress = false;
    this.associatedBillingAddresses = null;
    this.bIsServiceAddressValidated = false;
    this.oCurrentProviderLocationInfo = new providerLocationInfo();
  }

if (this.oCurrentProviderLocationInfo.phone) {
      this.oCurrentProviderLocationInfo.phone = this.oCurrentProviderLocationInfo.phone.replace(/\D/g, '');
    }
    if (this.oCurrentProviderLocationInfo.fax) {
      this.oCurrentProviderLocationInfo.fax = this.oCurrentProviderLocationInfo.fax.replace(/\D/g, '');
    }
