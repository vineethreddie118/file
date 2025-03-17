<div class="form-row">
    <div class="form-group col-12" style="display:inline-block">
        <label for="billingExpirationDate">Expiration Date for old location<span class="required-star"> *</span></label>
        <div class="input-group">
            <input type="text" style="max-width:10rem" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
                [ngClass]="{'red-border': expDate.invalid && (expDate.dirty || expDate.touched || formsubmitted)}"
                id="billingExpirationDate" name="billingExpirationDate" #expDate="ngModel"
                [(ngModel)]="replaceBillingExpirationDate"
                placeholder="mm/dd/yyyy"
                #dp6="ngbDatepicker" ngbDatepicker required />
            <i class="cal-icon far fa-calendar" (click)="dp6.toggle()"></i>
        </div>
        <div style="color:red" *ngIf="expDate.invalid && (expDate.dirty || expDate.touched || formsubmitted)">
            *Expiration Date is Required to replace a billing location
        </div>
        <div style="color:red" *ngIf="expDate.invalid && replaceBillingExpirationDate && (expDate.dirty || expDate.touched)">
            Invalid date
        </div>
    </div>
</div>
