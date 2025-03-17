<div class="form-group col-md-6 col-lg-4 col-xl-3">
    <label for="effectiveDateupdate">Effective Date<span class="required-star"> *</span></label>
    <div class="input-group">
        <input type="text" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
            [ngClass]="{'red-border': (effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched))}"
            id="effectiveDateupdate" name="effectiveDateupdate" #effectiveDate="ngModel"
            [ngModel]="selectedBillingAddress.effectiveDate | date: 'MM/dd/yyyy'"
            (ngModelChange)="selectedBillingAddress.effectiveDate = $event"
            placeholder="mm/dd/yyyy"
            #dp7="ngbDatepicker" ngbDatepicker required />
        <i class="cal-icon far fa-calendar" (click)="dp7.toggle()"></i>
    </div>
    <div style="color:red" *ngIf="effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched)">
        *Effective Date is Required.
    </div>
    <div style="color:red" *ngIf="effectiveDate.invalid && selectedBillingAddress.effectiveDate && (effectiveDate.dirty || effectiveDate.touched)">
        Invalid date.
    </div>
</div>
