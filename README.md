 if(this.oCurrentProviderLocationInfo.phone){
      this.oCurrentProviderLocationInfo.phone = this.oCurrentProviderLocationInfo.phone.replace(/\D/g,'');
    }
    if(this.oCurrentProviderLocationInfo.fax){
      this.oCurrentProviderLocationInfo.fax = this.oCurrentProviderLocationInfo.fax.replace(/\D/g,'');
    }

fnAddLocationDetails() {

    if(!this.servicetinclicked){
      this.addService.fnRaiseWarningDlg("Invalid"," Please click checkbox to add details");
      return;
    }

    if(!this.isbillingaddressclicked){
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
//   if (
//     this.oCurrentProviderLocationInfo.providerAlternateIDSection &&
//     this.oCurrentProviderLocationInfo.providerAlternateIDSection[0] &&
//     this.oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi.length === 0
// ) {
//     this.oCurrentProviderLocationInfo.providerAlternateIDSection = null;
// }

   
    this.fnAddLocation();
    this.showAccordians = false;

    const keysToDelete = [
      'billingAddress1', 'billingAddress2', 'billingZip', 'billingCity',
      'billingCounty', 'billingStateCode', 'billingDefaultCity',
      'billingAddressValidated', 'taxId', 'taxTypeId', 'payeeName',
      'providerSvcLocPayeeId', 'w9flag', 'effectiveDate', 'expirationDate'
    ];


    this.providerLocBillingInfo.forEach(location => {
      delete location.billingAddress1Current;
    })
     
   
    

    const providerLocation = new ProviderLocation();
    providerLocation.providerLocationInfo = this.oProviderLocation.providerLocationInfo.filter(locInfo => !locInfo.serviceLocationId || !locInfo.providerServiceLocationMapId)
    if(this.isservicemode){
      const selectedLocation = this.serviceLocations[this.selectedLocationIndex];
      let item:any = {};
      item.taxId=selectedLocation.taxId;
      item.taxTypeId=selectedLocation.taxTypeId;
      item.billingAddress1=selectedLocation.billingAddress1;
      item.billingAddress2=selectedLocation.billingAddress2;
      item.billingCity=selectedLocation.billingCity;
      item.billingStateCode=selectedLocation.billingStateCode;
    item.billingZip=  selectedLocation.billingZip;
    item.effectiveDate=selectedLocation.effectiveDate;
    item.expirationDate=selectedLocation.expirationDate;
      item.billingCounty=selectedLocation.billingCounty;
     item.billingDefaultCity= selectedLocation.billingDefaultCity;
     item.payeeName= selectedLocation.payeeName;
      item.providerSvcLocPayeeId=selectedLocation.providerSvcLocPayeeId;
     item.billingAddressValidated=selectedLocation.billingAddressValidated;
     item.providerSvcLocPayeeMapID=selectedLocation.providerSvcLocPayeeMapID;
      providerLocation.providerLocationInfo[0].providerLocBillingInfo =[item]
    } else if(providerLocation.providerLocationInfo.length > 0)
    providerLocation.providerLocationInfo[0].providerLocBillingInfo = this.providerLocBillingInfo.filter(billingInfo => !billingInfo.providerSvcLocPayeeId || billingInfo['isLocationByTin']);
    else if(this.selectedLocation){
      providerLocation.providerLocationInfo = this.oProviderLocation.providerLocationInfo.filter(locInfo => locInfo.providerServiceLocationMapId == this.selectedLocation)
      providerLocation.providerLocationInfo[0].providerLocBillingInfo = this.providerLocBillingInfo.filter(billingInfo => !billingInfo.providerSvcLocPayeeId || billingInfo['isLocationByTin']);
    
    } 
   
    providerLocation.providerLocationInfo.forEach(locationInfo => {
      if (locationInfo.providerSvcLocHours) {
        locationInfo.providerSvcLocHours.forEach(svcHour => {
          delete svcHour.day; // Delete the day property
           delete svcHour.showAddSymbol; // Delete the showAddSymbol property
        });
      }
    });

    providerLocation.providerLocationInfo.forEach(locationInfo => {
      if(locationInfo.providerSvcLocHours){
        locationInfo.providerSvcLocHours = locationInfo.providerSvcLocHours.filter(
          svcHour => !(svcHour.openTimeValue === svcHour.closeTimeValue)
        );
        }
        keysToDelete.forEach(key => {
          delete locationInfo[key];
        });
    });

    providerLocation.providerLocationInfo[0].providerLocBillingInfo.forEach(billinInfo =>{
      if(billinInfo['isLocationByTin']){
        delete billinInfo['isLocationByTin']
      }
    })

   
    //filter npi it that doesnot have any value
    providerLocation.providerLocationInfo[0].providerAlternateIDSection[0].providerNpi = 
    providerLocation.providerLocationInfo[0].providerAlternateIDSection[0].providerNpi.filter(providerNpi => providerNpi.npi);
  
    providerLocation.providerId = this.nProviderId;
    providerLocation.providerLocationInfo[0].providerAlternateIDSection[0].providerNpi.forEach((providerNpi) =>{
      providerNpi.providerTaxonomy = providerNpi.providerTaxonomy.filter(taxObj => taxObj.taxonomyCode )
      providerNpi.providerSvcLocNpiId = 0;
    if (providerNpi.providerTaxonomy.length >0 ) {
      providerNpi.providerTaxonomy = providerNpi.providerTaxonomy.map(taxonomy => ({
        id: this.getTaxonamtByCode(taxonomy) ,
        taxonomyCode: taxonomy?.taxonomyCode,
        isPrimary: taxonomy.isPrimary?true:false,
        active: taxonomy?.active ?true:false,
        isDelete: false
      }));
    }else{
      providerNpi.providerTaxonomy =[]; 
  
    }
    if (this.addService.eProviderType === ProviderTypeEnum.PRACTITIONER || !providerNpi.providerTaxonomy  ) {
      providerLocation.providerLocationInfo[0].providerAlternateIDSection = null;
    }
    else {
      if(!providerNpi.providerTaxonomy){
        providerLocation.providerLocationInfo[0].providerAlternateIDSection = null;
      }else{
       delete providerLocation.providerLocationInfo[0].providerAlternateIDSection[0].medicaidId ;
       delete providerLocation.providerLocationInfo[0].providerAlternateIDSection[0].medicare ;
      }
      //this.userId =null;
      //providerLocation.providerLocationInfo[0].providerAlternateIDSection = null;
    }
  })
   if(providerLocation.providerLocationInfo[0].providerAlternateIDSection && providerLocation.providerLocationInfo[0].providerAlternateIDSection[0].providerNpi.length === 0){
     providerLocation.providerLocationInfo[0].providerAlternateIDSection = null;
   }
    providerLocation.providerLocationInfo[0].isPrimary = this.isPrimary;

    // Handling Replace Location
    if (this.isReplaceMode) {

      //Setting new expiration Date
      this.handleLocationExpirationDate(this.replaceLocationExpirationDate, this.replaceIndex, null);

      //
      const providerLocationInfoBeingReplaced = {...this.oProviderLocation.providerLocationInfo[this.replaceIndex]};
      providerLocation.providerLocationInfo.push(providerLocationInfoBeingReplaced);
      providerLocation.providerLocationInfo[0].replaceLocId = providerLocationInfoBeingReplaced.serviceLocationId.toString();
      providerLocationInfoBeingReplaced.replaceLocId = providerLocationInfoBeingReplaced.serviceLocationId.toString();
      providerLocation.providerLocationInfo = providerLocation.providerLocationInfo.reverse();

      this.submitEditedProviderLocationInfo(providerLocation);

      //Resetting values to initial state
      this.replaceLocationExpirationDate = null;
      this.replaceIndex = -1;
      this.isReplaceMode = false;

    } else {
      this.fnSetInactiveLocationSectionVisibility(false);
      this.fnSetInactiveBillingLocationSectionVisibility(false);
      this.bIsLoading = true;
      this.addService.saveLocationInformation(providerLocation).subscribe((res: any) => {
        console.log(res)
        this.oCurrentProviderLocationInfo = new providerLocationInfo();
        this.bIsDirty = false;
        this.bIsLoading = false;
        this.serviceLocations =[]
        this.ServiceLocation="";
        this.bIsSearchResultsEmpty = false;
        this.bIsTinEmpty = false;
         this.searchperformed =false;
         this.showbillingaddressfields=true;
         
         this.isservicemode= false;
         this.selectedLocationIndex = null;
         this.fnFetchData();

      }, err => {
        this.bIsLoading = false;
        //this.addService.fnRaiseGenericAlert(JSON.stringify(err));
        //console.log(err);
        
        this.addService.fnRaiseGenericAlert('savePracticeLocation', err);
         this.oCurrentProviderLocationInfo = new providerLocationInfo();
         this.ServiceLocation='';
      this.serviceLocations=[];
      this.bIsSearchResultsEmpty = false;
      this.bIsTinEmpty = false;
       this.searchperformed =false;
       this.showbillingaddressfields=true;
    
       this.isservicemode= false;
       this.selectedLocationIndex = null;
       
         this.fnFetchData();
         
        
        
      })
    }

  }

    <div class="form-group col-md-6 col-lg-4 col-xl-3">
                    <label for="inputData">Phone </label>
                    <input type="tel" class="form-control form-control-custom" id="phone" name="phone" #phone="ngModel"
                        
                    [(ngModel)]="oCurrentProviderLocationInfo.phone"
                        [ngClass]="{'red-border': phone.invalid && (phone.dirty || phone.touched )}"
                        pattern="^\d{3}-\d{3}-\d{4}$" (input)="formatNumber($event,'phone')"  maxlength="12">

                    <div class="error-tooltip error-tooltip-offset"
                        *ngIf="phone.invalid && (phone.dirty || phone.touched )">
                      <ng-container *ngIf="phone.errors?.pattern">Invalid</ng-container>

                    </div>
                </div>

{
    "providerLocationInfo": [
        {
            "serviceLocationId": 0,
            "isDelete": false,
            "locationAltId": null,
            "serviceAddress": "8 1/2 4th Ave",
            "address2": "",
            "defaultCity": "Hudson Falls",
            "replaceLocId": "",
            "publicTransportationFlag": false,
            "handicappedAccessibleFlag": false,
            "locationAlternateXrefId": 0,
            "providerLocBillingInfo": [
                {
                    "billingAddress1": "87 1/2 8th St",
                    "billingAddress2": "",
                    "billingCity": "Fond du Lac",
                    "billingStateCode": "WI",
                    "billingZip": "54935-5054",
                    "billingDefaultCity": "Fond du Lac",
                    "billingCounty": "Fond du Lac",
                    "payeeName": "bjhb",
                    "taxId": "636868738",
                    "taxTypeId": "E",
                    "billingAddressValidated": true,
                    "w9flag": true,
                    "effectiveDate": "2025-03-21",
                    "expirationDate": null,
                    "providerSvcLocPayeeId": 0,
                    "providerSvcLocPayeeMapID": 0,
                    "isReplace": false,
                    "isDelete": false
                }
            ],
            "isPrimary": false,
            "providerAlternateIDSection": null,
            "providerSvcLocHours": [],
            "addressId": 0,
            "providerServiceLocationMapId": 0,
            "locationName": "testphone",
            "addressValidated": true,
            "city": "Hudson Falls",
            "stateCode": "NY",
            "zip": "12839-1930",
            "county": "Washington",
            "phone": "333-444-5555",
            "fax": "222-666-7777"
        }
    ],
    "providerId": 34077
}
