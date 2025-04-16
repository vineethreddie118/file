<form #npiForm="ngForm">
  <div class="form_field_outer">
    <div *ngFor="let npiAt of providerAlternateIDSection[0].providerNpi; let idx=index">
      <div class="form-row form_field_outer_row p-1">
        <div class="col-md-6 col-lg-4 col-xl-3">
          <div class="input-container">
            <label for="inputData">NPI</label>
            <input type="text" class="form-control form-control-custom" id="npi{{idx}}" name="npi{{idx}}"
                   #npi="ngModel" placeholder="NPI #" [(ngModel)]="npiAt.npi"
                   [ngClass]="{'red-border': npi.invalid && (npi.dirty || npi.touched)}" pattern="^\d{10}$"
                   maxlength="10" appTrimText>
          </div>
          <div class="error-tooltip error-tooltip-offset"
               *ngIf="npi.invalid && (npi.dirty || npi.touched)">
            <ng-container *ngIf="npi.errors?.pattern">Invalid</ng-container>
          </div>
        </div>
        <div class="col-md-6 col-lg-4 col-xl-3">
          <label for="inputData">Effective Date
          <span class="required-star" *ngIf="dc2.value"> *</span>
          </label>
          <div class="input-group">
            <input maxlength="10" dateFormater class="data-input form-control"
                   [ngClass]="{'red-border': ((!dc1.value && dc2.value) && dc1.invalid && (dc1.dirty || dc1.touched))}"
                   placeholder="mm/dd/yyyy" id="NpiEffDate{{idx}}" name="NpiEffDate{{idx}}" #dc1="ngModel"
                   [(ngModel)]="npiAt.effectiveDate" #dp1="ngbDatepicker" ngbDatepicker />
            <i class="cal-icon far fa-calendar" (click)="dp1.toggle()"></i>
          </div>
          <div class="error-tooltip">
            <ng-container *ngIf="!dc1.value && dc2.value">Required</ng-container>
            <ng-container *ngIf="dc1.value && dc1.invalid && (dc1.dirty || dc1.touched)">Invalid date</ng-container>
          </div>
        </div>
        <div class="col-md-6 col-lg-4 col-xl-3">
          <label for="inputData">Expiration Date</label>
          <div class="input-group">
            <input maxlength="10" dateFormater class="data-input form-control"
                   [ngClass]="{'red-border': (dc2.invalid && (dc2.dirty || dc2.touched ))}"
                   placeholder="mm/dd/yyyy" id="NpiExpDate{{idx}}" name="NpiExpDate{{idx}}" #dc2="ngModel"
                   [(ngModel)]="npiAt.expirationDate" #dp2="ngbDatepicker" ngbDatepicker />
            <i class="cal-icon far fa-calendar" (click)="dp2.toggle()"></i>
          </div>
          <div class="error-tooltip">
            <ng-container *ngIf="dc2.invalid && (dc2.dirty || dc2.touched)">Invalid date</ng-container>
            <ng-container *ngIf="dc2.value && IsInValidExpirationDate(dc1.value, dc2.value)">
              Expiration Date can not be less than Effective Date
            </ng-container>
          </div>
        </div>
      </div>
      <button type="button" class="btn btn-primaryicon btn-sm m-1 ml-4" (click)="addTaxonomyRow(idx)" data-toggle="tooltip"
              title="Add Taxonomy">
        <i class="fa-solid fa-plus"></i> Add Taxonomy
      </button>
      <div style="margin-left: 20px;">
        <div class="form-row form_field_outer_row p-1"
             *ngFor="let taxonomy of npiAt.providerTaxonomy; let i = index">
          <div class="col-md-6 col-lg-4 col-xl-3">
            <label for="inputData">Taxonomy Code</label>
            <select class="form-control form-control-custom" id="Taxonomy{{i}}" name="Taxonomy{{i}}"
                    #Taxonomy="ngModel" [(ngModel)]="taxonomy.taxonomyCode" (change)="validateForPrimaryExist()">
              <option value=""></option>
              <ng-container *ngFor="let providerType of taxonomyCodes">
                <option [ngValue]="providerType.taxonomyCode">
                  {{providerType.taxonomyCode}}
                </option>
              </ng-container>
            </select>
          </div>
          <div class="col-md-6 col-lg-4 col-xl-3">
            <label for="inputData">
              Effective Date
              <span class="required-star" *ngIf="dc4.value"> *</span>
            </label>
            <div class="input-group">
              <input maxlength="10" dateFormater class="data-input form-control"
                     id="TaxonomyEffDate{{i}}"
                     name="TaxonomyEffDate{{i}}"
                     [ngClass]="{'red-border': ((!dc3.value && dc4.value) && dc3.invalid && (dc3.dirty || dc3.touched))}"
                     placeholder="mm/dd/yyyy" name="effectiveDate" #dc3="ngModel"
                     [(ngModel)]="taxonomy.effectiveDate" #dp3="ngbDatepicker"
                     ngbDatepicker/>
              <i class="cal-icon far fa-calendar" (click)="dp3.toggle()"></i>
            </div>
            <div class="error-tooltip">
              <ng-container *ngIf="!dc3.value && dc4.value">Required</ng-container>
              <ng-container *ngIf="dc3.value && dc3.invalid && (dc3.dirty || dc3.touched)">
                Invalid date
              </ng-container>
              <ng-container *ngIf="dc3.value && !dc3.invalid && dc3.value && !dc3.invalid && IsInValidExpirationDate(dc1.value, dc3.value)">
                Taxonomy effective date can not be earlier than the NPI effective date
              </ng-container>
              <ng-container *ngIf="dc3.value && !dc3.invalid && dc3.value && !dc3.invalid && IsInValidExpirationDate(dc3.value, dc2.value)">
                Taxonomy effective date can not be later than the NPI expiration date
              </ng-container>
            </div>
          </div>
          <div class="col-md-6 col-lg-4 col-xl-3">
            <label for="inputData">Expiration Date</label>
            <div class="input-group">
              <input maxlength="10" dateFormater class="data-input form-control"
                     id="TaxonomyExpDate{{i}}"
                     name="TaxonomyExpDate{{i}}"
                     [ngClass]="{'red-border': (dc4.invalid && (dc4.dirty || dc4.touched ))}"
                     placeholder="mm/dd/yyyy" name="expirationDate" #dc4="ngModel"
                     [(ngModel)]="taxonomy.expirationDate" #dp4="ngbDatepicker" (ngModelChange)="onExpirationDateChange($event,i, idx)"
                     ngbDatepicker />
              <i class="cal-icon far fa-calendar" (click)="dp4.toggle()"></i>
            </div>
            <div class="error-tooltip">
              <div>
                <ng-container *ngIf="dc4.invalid && (dc4.dirty || dc4.touched)">
                  Invalid date
                </ng-container>
              </div>
              <div>
                <ng-container *ngIf="dc4.value && !dc4.invalid && dc3.value && !dc3.invalid && IsInValidExpirationDate(dc3.value, dc4.value)">
                  Expiration Date can not be less than Effective Date
                </ng-container>
              </div>
              <div>
                <ng-container *ngIf="dc4.value && !dc4.invalid && dc2.value && !dc2.invalid && compareExpirationDates(dc2.value, dc4.value)">
                  Taxonomy expiration date can not be later than the NPI expiration date
                </ng-container>
              </div>
            </div>
          </div>
          <div class="col-md-3 col-lg-2 col-xl-2 isPrimaryCheck">
            <label class="f1 font-weight-bold">Primary</label>
            <input class="form-check-input" type="checkbox" id="checkboxTrue" name="isPrimary{{i}}"
                   [(ngModel)]="taxonomy.isPrimary" (change)="onIsPrimaryChange($event,i,idx)">
          </div>
        </div>
      </div>
      <div class="spacer-divider" *ngIf="providerAlternateIDSection[0].providerNpi.length > 1"></div>
    </div>
  </div>
</form>

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

    if(!this.isNpiTaxonomyValid) {
      this.addService.fnRaiseWarningDlg(
        'Invalid NPI/Taxonomy',
        'Please enter valid NPI/Taxonomy.'
      );
      return;
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
          effectiveDate: taxonomy.effectiveDate,
          expirationDate: taxonomy.expirationDate,
          providerNPIID: taxonomy.providerNPIID
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
            providerNPIID: this.providerNpi.providerNPIID
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
              if (!this.providerForm.providerId) {
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

  getTaxonamtByCode(taxonamy) {
    const filteredTaxonamy = this.providerLookupDetails.taxonomyCodes.filter(
      (taxonamyObj) => taxonamyObj.taxonomyCode === taxonamy.taxonomyCode
    );
    return filteredTaxonamy[0].id;
  }
export class ProviderAlternateIDSection {
    providerNpi: ProviderNpi[] = [];
    medicaidId = null;
    medicare: Medicare = null

}

export class ProviderNpi {
    providerSvcLocNpiId: number;
    npi: string | undefined;
    effectiveDate: Date | string;
    expirationDate: Date | string;
    serviceLocationId: number = 0;
    providerNPIID: number = 0;
    providerTaxonomy: ProviderTaxonomy[];
}
export class ProviderTaxonomy {
    id: number;
    taxonomyCode: string;
    isPrimary: boolean;
    active: boolean;
    isDelete: boolean;
    providerNPIID: number = 0;
    effectiveDate?: Date | string;
    expirationDate?: Date | string;
}
