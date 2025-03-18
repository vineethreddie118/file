<div class="form-row" *ngIf="isReplaceMode">
    <div class="form-group col-12" style="display:inline-block">
        <label for="locationExpirationDate">Expiration Date<span class="required-star"> *</span></label>
        <div class="input-group" style="max-width:10rem">
            <input type="text" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
                [ngClass]="{'red-border': repExpDate.invalid && (repExpDate.dirty || repExpDate.touched || formsubmitted)}"
                id="locationExpirationDate" name="locationExpirationDate" #repExpDate="ngModel"
                [ngModel]="replaceLocationExpirationDate | date: 'MM/dd/yyyy'"
                (ngModelChange)="replaceLocationExpirationDate = $event"
                placeholder="mm/dd/yyyy"
                #dpReplace="ngbDatepicker" ngbDatepicker required />
            <i class="cal-icon far fa-calendar" (click)="dpReplace.toggle()"></i>
        </div>
        <div style="color:red" *ngIf="repExpDate.invalid && (repExpDate.dirty || repExpDate.touched || formsubmitted)">
            Expiration Date is Required to replace a location
        </div>
        <div style="color:red" *ngIf="repExpDate.invalid && replaceLocationExpirationDate && (repExpDate.dirty || repExpDate.touched)">
            Invalid date.
        </div>
    </div>
</div>
