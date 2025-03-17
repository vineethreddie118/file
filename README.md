<div class="form-group" style="display:inline-flex; margin-top: 12px;">
    <label for="newEffectiveDate">Effective Date<span class="required-star"> *</span></label>
    <div class="input-group" style="max-width:50% !important; margin-left: 3% !important">
        <input type="text" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
            [ngClass]="{'red-border': (newEffectiveDate.invalid && (newEffectiveDate.dirty || newEffectiveDate.touched))}"
            id="newEffectiveDate" name="newEffectiveDate" #newEffectiveDate="ngModel"
            [ngModel]="oLocationToReactivate.newEffectiveDate | date: 'MM/dd/yyyy'"
            (ngModelChange)="oLocationToReactivate.newEffectiveDate = $event"
            placeholder="mm/dd/yyyy"
            #dp9="ngbDatepicker" ngbDatepicker required />
        <i class="cal-icon far fa-calendar" (click)="dp9.toggle()"></i>
    </div>
</div>

<!-- Existing Error Message -->
<div *ngIf="false === oLocationToReactivate?.bIsValidEffectiveDate && oLocationToReactivate?.bIsReactivateClicked"
    style="color: #ff0000;">
    The reactivate effective date is required and cannot be on or earlier than the previous expiration date ({{oLocationToReactivate.oldExpirationDate}}).
</div>

<!-- New Invalid Date Error Message -->
<div style="color: #ff0000;" *ngIf="newEffectiveDate.invalid && oLocationToReactivate.newEffectiveDate && (newEffectiveDate.dirty || newEffectiveDate.touched)">
    Invalid date.
</div>
