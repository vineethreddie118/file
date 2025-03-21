 <div class="form-group col-md-6 col-lg-4 col-xl-3">
                        <label for="expirationDate">Expiration Date</label>
                        <div class="input-group">
                            <input maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
                                [ngClass]="{'red-border': ( expirationDate.invalid && (expirationDate.dirty || expirationDate.touched|| formsubmitted ))}"
                                placeholder="mm/dd/yyyy" name="expirationDate" #expirationDate="ngModel"
                                [(ngModel)]="oCurrentProviderLocationInfo.expirationDate"
                                #dp2="ngbDatepicker" ngbDatepicker />
                            <i class="cal-icon far fa-calendar" (click)="dp2.toggle()"></i>
                            <div class="error-tooltip">
                                <ng-container *ngIf="expirationDate.invalid && (expirationDate.dirty || expirationDate.touched)">Invalid date</ng-container>
                            </div>
                        </div>
                    </div>

public fnAddBillingAddress(): void {

    

    if ( !this.bIsBillingAddressValidated && !this.bIsInvalidBillingAddressAllowed){
      this.addService.fnRaiseWarningDlg("Invalid" ,"Please enter a valid address or accept to save an invalid address (More information is provided below Validate Address button)");
      return;
    }
    if (this.bIsBillingAddressValidated || this.bIsInvalidBillingAddressAllowed) {
      
    
      if (this.locationsByTin.length > 0) {
        this.locationsByTin.forEach(location => {
          if (location.selected) {
            delete location.selected
        

            this.providerLocBillingInfo.push(location)
            this.locationsByTin = [];
            this.Tin = null;
            this.showbillingaddressfields = false;
            this.isbillingaddressclicked = true;
          }
        })
        
      } else if (this.oCurrentProviderLocationInfo.billingAddress1) {
        this.oCurrentProviderLocationInfo.billingAddress1Current={
          "street_line":this.oCurrentProviderLocationInfo.billingAddress1,
          "secondary": this.oCurrentProviderLocationInfo.billingAddress2,
          "city": this.oCurrentProviderLocationInfo.billingCity,
          "state": this.oCurrentProviderLocationInfo.billingStateCode,
          "zipcode": this.oCurrentProviderLocationInfo.billingZip,
      }
       

        this.billingAddressButton = true;
        
        this.formsubmitted = true;
        if (this.practiceform.valid) {
          let oCurrentProviderLocationInfo = JSON.parse(JSON.stringify(this.oCurrentProviderLocationInfo))
          delete oCurrentProviderLocationInfo.billingAddress1Current;
          

          oCurrentProviderLocationInfo.billingAddressValidated = this.bIsBillingAddressValidated;
          

          if (-1 === this.nEditIndex || this.isEditMode) {
            
            this.providerLocBillingInfo.push({
              billingAddress1Current: this.oCurrentProviderLocationInfo.billingAddress1Current,
              billingAddress1: this.oCurrentProviderLocationInfo.billingAddress1,
              billingAddress2: this.oCurrentProviderLocationInfo.billingAddress2,
              billingCity: this.oCurrentProviderLocationInfo.billingCity,
              billingStateCode: this.oCurrentProviderLocationInfo.billingStateCode,
              billingZip: this.oCurrentProviderLocationInfo.billingZip,
              billingDefaultCity: this.oCurrentProviderLocationInfo.billingDefaultCity,
              billingCounty: this.oCurrentProviderLocationInfo.billingCounty,
              payeeName: this.oCurrentProviderLocationInfo.payeeName,
              taxId: this.oCurrentProviderLocationInfo.taxId,
              taxTypeId: this.oCurrentProviderLocationInfo.taxTypeId,
              billingAddressValidated: this.bIsBillingAddressValidated,
              w9flag: true,
              effectiveDate: this.oCurrentProviderLocationInfo.effectiveDate,
              expirationDate: this.oCurrentProviderLocationInfo.expirationDate ? this.oCurrentProviderLocationInfo.expirationDate:null,
              providerSvcLocPayeeId: this.oCurrentProviderLocationInfo.providerSvcLocPayeeId,
              providerSvcLocPayeeMapID:0,
              isReplace: false,
              isDelete: false,
            });
         

          } else {
            Object.assign(
              this.oProviderLocation.providerLocationInfo[this.nEditIndex],
              this.oCurrentProviderLocationInfo
            );
            this.nEditIndex = -1;
            this.isEditMode = false;
         

          }
          this.isbillingaddressclicked =true;
          this.iseditaddmode = false;
          this.resetBillingAddress();
          this.formsubmitted = false;
          this.bIsDirty = true;
          this.bIsAddclicked = true;

          this.showbillingaddressfields = false;



          Object.keys(this.practiceform.controls).forEach((key) => {
            const control = this.practiceform.controls[key];
            control.markAsPristine();
            control.markAsUntouched();
          })
          this.bIsServiceAddressValidated = true;

          this.practiceform.controls['sameAsServiceAddress'].setValue(false);
          this.Tin = null;
          this.locationsByTin = [];
          
        }
        this.bIsBillingAddressValidated = true;

        //this.oCurrentProviderLocationInfo.providerSvcLocHours = officeHours;
      }
    }
  }

 showaddedbillingfields() {
    
    this.showbillingaddressfields = true;
    this.isaddmorebillclicked = true;
    this.bIsBillingAddressValidated = false;
    this.isbillingaddressclicked = false;
    this.iseditaddmode = true;
    
    
  }
