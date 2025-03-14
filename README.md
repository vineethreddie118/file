<div class="modal-body" style="display:inline-flex">
      <div class="form-group col-12" style="display:inline-flex">
        <label for="locationExpirationDate">Expiration Date<span class="required-star"> *</span></label>
        <input style="max-width:50% !important; margin-left: 3% !important" type="date" class="form-control form-control-custom"
          id="locationExpirationDate" name="locationExpirationDate" [(ngModel)]="sLocationExpirationDate" required />
        <div style="display:inline-flex; margin-top:1% !important; margin-right:25% !important"
          class="error-tooltip error-tooltip-offset" *ngIf="!sLocationExpirationDate && isTerminateBtnClicked">
          Expiration Date is Required to terminate {{isBillingNotProvider ? 'billing' : 'practice'}} location
        </div>
      </div>
    </div>

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
