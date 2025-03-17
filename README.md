<div class="form-group col-md-6 col-lg-4 col-xl-3">
    <label for="effectiveDate">Effective Date<span class="required-star"> *</span></label>
    <div class="input-group">
        <input type="text" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
            [ngClass]="{'red-border': (effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched || formsubmitted))}"
            placeholder="mm/dd/yyyy" name="effectiveDate" #effectiveDate="ngModel"
            [ngModel]="oCurrentProviderLocationInfo.effectiveDate | date: 'MM/dd/yyyy'"
            (ngModelChange)="oCurrentProviderLocationInfo.effectiveDate = $event"
            #dp4="ngbDatepicker" ngbDatepicker required />
        <i class="cal-icon far fa-calendar" (click)="dp4.toggle()"></i>
        <div class="error-tooltip" *ngIf="effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched || formsubmitted)">
            Required
        </div>
    </div>
</div>
