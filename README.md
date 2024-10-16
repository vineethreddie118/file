 <div class="form-group col-8">
                <label for="inputData">Mailing Address<span class="required-star"> *</span></label>
                <div class="input-container">
                    <input type="text" class="form-control form-control-custom" id="address1" name="address1" #address1="ngModel"
                        [ngClass]="{'red-border':(address1.invalid && (address1.dirty || address1.touched || formsubmitted))}"
                        placeholder="Type to get suggestions..."  maxlength="100"
                        [ngClass]="{'red-border': ( address1.invalid && (address1.dirty || address1.touched || formsubmitted ))}"
                        [(ngModel)]="oSelectedAddress" [ngbTypeahead]="fnGetSmartyStSuggestions"
                        (selectItem)="fnHandleAddressChange($event)" (input)="fnHandleAddressChange($event)" [resultTemplate]="rt"
                        [inputFormatter]="fnSmartyStAddressFormatter" (change)="onChange($event)" [editable]="false">

                    <small *ngIf="bIsSearchingForAddress" class="form-text text-muted search-info">searching...</small>

                    <div class="error-tooltip"
                        *ngIf="address1.invalid && (address1.dirty || address1.touched || formsubmitted)">
                        Required
                    </div>
                </div>
            </div>

 <div class="form-group col-12">
            <app-location-info
                [bIsValidAddress]="bIsLocationValidated"
                [oAddressChanged]="oAddressChanged"
                [bDisableBtn]="address1.invalid || city.invalid || stateCode.invalid || zip.invalid"
                [oSmartyStInfo]="fnGetSmartyStInfo()"
                (addressUpdatedInfo)="fnProfileHandleSmartyStAddressUpdatedEvent($event)"
                (invalidAddressUserChoice)="fnHandleInvalidAddressUserChoice($event)">
            </app-location-info>
        </div>


  public fnProfileHandleSmartyStAddressUpdatedEvent(oUpdatedAddressInfo: SmartyStreetUpdatedInfo) {
    this.bIsMissingSecondaryInfo = oUpdatedAddressInfo.bIsMissingSecondaryInfo;
    this.bIsLocationValidated = oUpdatedAddressInfo.bIsServiceAddressValidated;

    if (!!oUpdatedAddressInfo.street) {
      this.providerForm.address1 = oUpdatedAddressInfo.street;
      this.oSelectedAddress = {...this.oSelectedAddress, street_line: this.providerForm.address1};
    }

    if (!!oUpdatedAddressInfo.street) {
      this.providerForm.address2 = oUpdatedAddressInfo.street2;
      this.oSelectedAddress = {...this.oSelectedAddress, street2: this.providerForm.address2};
    }

    if (!!oUpdatedAddressInfo.city)
      this.providerForm.city = oUpdatedAddressInfo.city;

    if (!!oUpdatedAddressInfo.zip)
      this.providerForm.zip = oUpdatedAddressInfo.zip;

    if (!!oUpdatedAddressInfo.county)
      this.providerForm.county = oUpdatedAddressInfo.county;
  }

<div class="form-group col-8">
                      <label for="inputData">Service Address<span class="required-star"> *</span></label>
                      <div class="input-container">
                        <input type="text" class="form-control form-control-custom" id="serviceAddress" name="serviceAddress" #serviceAddress="ngModel"
                          [ngClass]="{'red-border':(serviceAddress.invalid && (serviceAddress.dirty || serviceAddress.touched || formsubmitted))}"
                          placeholder="Type to get suggestions..." [(ngModel)]="oCurrentProviderLocationInfo.serviceAddressCurrent"
                          maxlength="25" [ngbTypeahead]="fnGetSmartyStSuggestions" (selectItem)="fnHandleAddressChange($event )"
                          (input)="fnHandleAddressChange($event)" [resultTemplate]="rt" [inputFormatter]="fnSmartyStAddressFormatter"
                          [editable]="false" (change)="onChange($event)">
                        <small *ngIf="bIsSearchingForAddress" class="form-text text-muted search-info">searching...</small>
                        <div class="error-tooltip"
                          *ngIf="serviceAddress.invalid && (serviceAddress.dirty || serviceAddress.touched || formsubmitted)">
                          Required
                        </div>
                      </div>

 public fnHandleSmartyStAddressUpdatedEvent(oUpdatedAddressInfo: SmartyStreetUpdatedInfo) {
   this.bIsMissingSecondaryInfo = oUpdatedAddressInfo.bIsMissingSecondaryInfo;
   this.bIsServiceAddressValidated= oUpdatedAddressInfo.bIsServiceAddressValidated;
   this.bIsLocationValidated= oUpdatedAddressInfo.bIsServiceAddressValidated;
   //this.bIsBillingAddressValidated = oUpdatedAddressInfo.bIsBillingAddressValidated;

   if (!!oUpdatedAddressInfo.street)
   this.oCurrentProviderLocationInfo.serviceAddress = oUpdatedAddressInfo.street;

   if (!!oUpdatedAddressInfo.city)
     this.oCurrentProviderLocationInfo.city = oUpdatedAddressInfo.city;

   if (!!oUpdatedAddressInfo.zip)
     this.oCurrentProviderLocationInfo.zip = oUpdatedAddressInfo.zip;

   if (!!oUpdatedAddressInfo.county)
     this.oCurrentProviderLocationInfo.county = oUpdatedAddressInfo.county;

     this.bIsInvalidAddressAllowed = this.bIsServiceAddressValidated;
     this.oCurrentProviderLocationInfo.addressValidated = oUpdatedAddressInfo.bIsServiceAddressValidated;
 }
                
                    
  public fnHandleAddressChange(e: any): void {
    this.oCurrentProviderLocationInfo.serviceAddressCurrent=this.oCurrentProviderLocationInfo.serviceAddressCurrent || {};
    if (e.item) {
      // If the address is selected from suggestions, populate the fields accordingly
      this.oCurrentProviderLocationInfo.serviceAddress = e.item.street_line.length >25 ?e.item.street_line.substring(0,24):e.item.street_line;
      this.oCurrentProviderLocationInfo.address2 =e.item.street_line.length >25 ?e.item.street_line.substring(25,e.item.street_line.length): e.item.secondary,
      this.oCurrentProviderLocationInfo.city = e.item.city;
      this.oCurrentProviderLocationInfo.stateCode = e.item.state;
      this.oCurrentProviderLocationInfo.zip = e.item.zipcode;
      this.oCurrentProviderLocationInfo.defaultCity = e.item.city;
     

      this.oCurrentProviderLocationInfo.serviceAddressCurrent = {
        "street_line":e.item.street_line.length >25 ?e.item.street_line.substring(0,24):e.item.street_line,
        "address2":e.item.street_line.length >25 ?e.item.street_line.substring(25,e.item.street_line.length): e.item.secondary,
        "city": e.item.city,
        "stateCode": e.item.state,
        "zip": e.item.zipcode
    };
      
  } else {
      // If the address is manually entered, use the entered address and clear other fields
      this.oCurrentProviderLocationInfo.serviceAddress = e.target.value;
      //this.oCurrentProviderLocationInfo.serviceAddressCurrent.street_line=e.target.value;
      this.oCurrentProviderLocationInfo.serviceAddressCurrent = {
        "street_line": e.target.value,
        "address2": this.oCurrentProviderLocationInfo.address2 || '',
        "city": this.oCurrentProviderLocationInfo.city || '',
        "stateCode": this.oCurrentProviderLocationInfo.stateCode || '',
        "zip": this.oCurrentProviderLocationInfo.zip || ''
    };
  }

    this.bIsServiceAddressValidated = false;
    this.oAddressChanged = {...this.oAddressChanged, val: true};
    this.checkIfAddressesAreSame();
  }
