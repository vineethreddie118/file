<div class="form-group col-12" style="display:inline-flex">
    <label for="locationExpirationDate">Expiration Date<span class="required-star"> *</span></label>
    <div class="input-group" style="max-width:50% !important; margin-left: 3% !important">
        <input type="text" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
            [ngClass]="{'red-border': (locationExpirationDate.invalid && (locationExpirationDate.dirty || locationExpirationDate.touched))}"
            id="locationExpirationDate" name="locationExpirationDate" #locationExpirationDate="ngModel"
            [ngModel]="sLocationExpirationDate | date: 'MM/dd/yyyy'"
            (ngModelChange)="sLocationExpirationDate = $event"
            placeholder="mm/dd/yyyy"
            #dp8="ngbDatepicker" ngbDatepicker required />
        <i class="cal-icon far fa-calendar" (click)="dp8.toggle()"></i>
    </div>
    <div style="display:inline-flex; margin-top:1% !important; margin-right:25% !important; color:red"
        class="error-tooltip error-tooltip-offset"
        *ngIf="!sLocationExpirationDate && isTerminateBtnClicked">
        Expiration Date is Required to terminate {{isBillingNotProvider ? 'billing' : 'practice'}} location
    </div>
    <div style="display:inline-flex; margin-top:1% !important; margin-right:25% !important; color:red"
        class="error-tooltip error-tooltip-offset"
        *ngIf="locationExpirationDate.invalid && sLocationExpirationDate && (locationExpirationDate.dirty || locationExpirationDate.touched)">
        Invalid date.
    </div>
</div>
