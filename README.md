import { Directive, ElementRef, HostListener } from '@angular/core'

@Directive({
    selector: '[dateFormater]'
})
export class DateFormatterDirective {

    constructor(private el: ElementRef) { }

    @HostListener('input', ['$event'])
    onKeyDown(event: KeyboardEvent) {
        const input = event.target as HTMLInputElement;

        const preVal = input.value;

        input.value =
            input.value.replace(/^(\d\d)(\d)$/g, '$1/$2').replace(/^(\d\d\/\d\d)(\d+)$/g, '$1/$2').replace(/[^\d\/]/g, '');

        // let out = input.value.replace(/\D/g, '');
        // let len = out.length;

        // if (len > 1 && len < 4) {
        //     out = out.substring(0, 2) + '-' + out.substring(2, 3);
        // } else if (len >= 4) {
        //     out = out.substring(0, 2) + '-' + out.substring(2, 4) + '-' + out.substring(4, len);
        //     out = out.substring(0, 10)
        // }
        // input.value = out;

        // let trimmed = input.value.replace(/\s+/g, '');

        // if (trimmed.length > 10) {
        //     trimmed = trimmed.substr(0, 10);
        // }

        // trimmed = trimmed.replace(/-/g, '');

        // let numbers = [];

        // numbers.push(trimmed.substr(0, 2));
        // if (trimmed.substr(2, 2) !== "")
        //     numbers.push(trimmed.substr(2, 2));
        // if (trimmed.substr(4, 4) != "")
        //     numbers.push(trimmed.substr(4, 4));

        // input.value = numbers.join('-');
    }

    @HostListener('paste', ['$event'])
    onPaste(event: ClipboardEvent) {
        event.preventDefault();
        const pastedValue = event.clipboardData?.getData('text') || '';

        const cleanedVal = pastedValue.replace(/[-\/]/g, '');

        if(cleanedVal.length !== 8) return;

        let trimmed = cleanedVal.replace(/\s+/g, '');

        if (trimmed.length > 10) {
            trimmed = trimmed.substr(0, 10);
        }

        trimmed = trimmed.replace(/-/g, '');

        let numbers = [];

        numbers.push(trimmed.substr(0, 2));
        if (trimmed.substr(2, 2) !== "")
            numbers.push(trimmed.substr(2, 2));
        if (trimmed.substr(4, 4) != "")
            numbers.push(trimmed.substr(4, 4));

        this.el.nativeElement.value = numbers.join('/');
        this.el.nativeElement.dispatchEvent(new Event('input', { bubbles: true }));
    }
}

 <div  *ngIf="eProviderType === ProviderTypeEnum.PRACTITIONER" class="col-md-6 col-lg-4 col-xl-3">
                        <label for="inputData">Date of Birth</label>
                        <div class="input-group">
                            <input maxlength="10" dateFormater class="data-input form-control" [ngClass]="{'red-border': (dc1.invalid && (dc1.dirty || dc1.touched ))}"
                                placeholder="mm/dd/yyyy" name="dob" #dc1="ngModel"
                                [(ngModel)]="providerForm.dob" #dp="ngbDatepicker" 
                                ngbDatepicker [maxDate]="currentDate"/>
                            <i class="cal-icon far fa-calendar" (click)="dp.toggle()"></i>
                            <div class="error-tooltip">
                                <ng-container *ngIf="dc1.invalid && (dc1.dirty || dc1.touched)">Invalid date</ng-container>
                            </div>
                        </div>
                    </div>

 public fnNextBtnClick(): void {
    this.formsubmitted = true;

    if (!this.profileForm.valid) {
      return;
    }

    if (false === this.fnIsAddressValid()) {
      this.addService.fnRaiseWarningDlg(
        'Invalid',
        ' Please enter valid address'
      );
      return;
    }
    if (!this.providerForm.phone && this.providerForm.phoneExt) {
      this.addService.fnRaiseWarningDlg(
        'Missing Phone',
        'Please enter the phone number.'
      );
      return;
    }
    if (this.providerForm.providerTypeId !== ProviderTypeEnum.GROUP) {
      let effectiveDate = this.providerForm.providerAlternateIDSection[0].providerNpi[0].effectiveDate;
      let expirationDate = this.providerForm.providerAlternateIDSection[0].providerNpi[0].expirationDate;

      // Convert dates if they are in string format
      if (typeof effectiveDate === 'string') {
        effectiveDate = new Date(effectiveDate);
      }
      if (typeof expirationDate === 'string') {
        expirationDate = new Date(expirationDate);
      }

     
      if (!effectiveDate && expirationDate) {
        this.addService.fnRaiseWarningDlg(
          'Missing Effective Date',
          'Please enter the effective date.'
        );
        return;
      }

    
      if (effectiveDate && expirationDate && effectiveDate > expirationDate) {
        this.addService.fnRaiseWarningDlg(
          'Invalid Date',
          'Effective Date cannot be later than Expiration Date.'
        );
        return;
      }
    }

    this.bIsLoading = true;

    if(this.providerForm.phone){
      this.providerForm.phone = this.providerForm.phone.replace(/\D/g,'');
    }
    if(this.providerForm.fax){
      this.providerForm.fax = this.providerForm.fax.replace(/\D/g,'');
    }

    const id = !!this.nProviderId ? this.nProviderId : 0;
    if (!this.alternateIdActive) {
      this.alternateIdActive = 'ACTIVE';
    }

    const requestTaxnomy = [];
    if (
      this.providerForm.providerTypeId === ProviderTypeEnum.GROUP ||
      !this.providerForm.providerAlternateIDSection[0].providerNpi[0].npi
    ) {
      this.providerForm.providerAlternateIDSection = null;
    } else {
      this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy =
        this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy.filter(
          (taxObj) => taxObj.taxonomyCode
        );
      for (const taxonomy of this.providerForm.providerAlternateIDSection[0]
        .providerNpi[0].providerTaxonomy) {
        const requestTaxonomy = {
          id: this.getTaxonamtByCode(taxonomy),
          taxonomyCode: taxonomy?.taxonomyCode,
          isPrimary: taxonomy.isPrimary ? true : false,
          active: taxonomy?.active === true ? true : false,
          isDelete: false,
        };
        requestTaxnomy.push(requestTaxonomy);
      }
      this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy =
        requestTaxnomy;
    }

    this.providerForm['secondaryLicenseCodeID'] =
      this.providerForm['secondaryLicenseCodeID'] || null;
    this.providerForm['secondaryLicenseCodeDescription'] =
      this.providerForm['secondaryLicenseCodeDescription'] || null;
    this.providerForm['raceId'] = this.providerForm['raceId'] || null;
    this.providerForm['raceName'] =
      this.getRaceNameById(this.providerForm['raceId']) || null;
    this.providerForm['ethnicityId'] = this.providerForm['ethnicityId'] || null;
    this.providerForm['ethnicityName'] =
      this.getethinicityNameById(this.providerForm['ethnicityId']) || null;
    this.providerForm['providerContactTypeID'] =
      this.providerForm['providerContactTypeID'] || null;
    this.providerForm['email'] = this.providerForm['email'] || null;
    this.providerForm['caqhid'] = this.providerForm['caqhid'] || null;
    // this.providerForm["providerContactName"] = this.getcontactNameById(this.providerForm["providerContactTypeID"]) || null;
    this.providerForm['savetoFlexcare'] = true;
    this.providerForm['addressValidated'] = this.fnIsAddressValid();
    this.providerForm['userId'] = this.oSearchformSvc?.userDetail?.userID || 2;
    this.fnCorrectDateFields();
    const save = () => {
      this.addService.saveProfileInformation(id, this.providerForm).subscribe(
        (res: any) => {
          if (!this.nProviderId) {
            this.providerForm.providerId = res.newProviderId;
            this.updateProviderIdEvent.next(this.providerForm);
          }
          // this.hasChanges = false;
          this.bIsLoading = false;
          this.navigationEvent.next({
            enumCurrentScreen: ScreenType.PROFILE,
            bIsNext: true,
          });
          this.providerForm.providerAlternateIDSection = [];
          this.providerTaxonamy = {
            id: 0,
            taxonomyCode: '',
            isPrimary: false,
            active: true,
            isDelete: false,
          };
          this.providerNpi.providerTaxonomy = [this.providerTaxonamy];

          this.providerForm.providerAlternateIDSection.push({
            providerNpi: [this.providerNpi],
            medicaidId: null,
            medicare: this.medicare,
          });
        },
        (err) => {
          //this.addTaxonomyRow();

          if (!this.nProviderId) {
            this.providerForm.providerAlternateIDSection = [];
            this.providerNpi.providerTaxonomy = [this.providerTaxonamy];
            this.providerForm.providerAlternateIDSection.push({
              providerNpi: [this.providerNpi],
              medicaidId: null,
              medicare: this.medicare,
            });
          }
          if (!this.providerForm.providerAlternateIDSection) {
            this.providerForm.providerAlternateIDSection = [];

            this.providerForm.providerAlternateIDSection.push({
              providerNpi: [this.providerNpi],
              medicaidId: null,
              medicare: this.medicare,
            });
          }

          this.bIsLoading = false;
          this.addService
            .fnRaiseGenericAlert('saveProfileInformation', err)
            .result.then((closeResult) => {
              const nUPDId = this.addService.fnIsUPDInsertSuccess(err);
              if (0 != nUPDId) {
                this.providerForm.providerId = nUPDId;

                this.updateProviderIdEvent.next(this.providerForm);
              }
              if (this.providerForm.providerId === 0) {
                this.addService.fnRaiseGenericAlert(
                  'Provider Creation failed in UPD.',
                  err
                );
                return;
              }
              this.navigationEvent.next({
                enumCurrentScreen: ScreenType.PROFILE,
                bIsNext: true,
              });
            });
        }
      );
    };
    if (!this.providerForm.casid || !this.providerForm.flexcareID) {
      this.providerForm.hasModifiedProfileInfo = true;
      this.providerForm.hasModifiedProviderNpi = true;
      this.providerForm.hasModifiedProviderTaxonomy = true;

      save();
    } else if (this.bIsDirty || this.fnIsDirty()) {
      save();
    } else {
      // this.updateProviderIdEvent.next(this.providerForm);
      this.navigationEvent.next({
        enumCurrentScreen: ScreenType.PROFILE,
        bIsNext: true,
      });
    }
  }
