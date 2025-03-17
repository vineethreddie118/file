<div class="form-group col-md-6 col-lg-4 col-xl-3">
    <label for="expirationDateupdate">Expiration Date</label>
    <div class="input-group">
        <input type="text" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
            [ngClass]="{'red-border': (expirationDate.invalid && (expirationDate.dirty || expirationDate.touched))}"
            id="expirationDateupdate" name="expirationDateupdate" #expirationDate="ngModel"
            [ngModel]="selectedBillingAddress.expirationDate | date: 'MM/dd/yyyy'"
            (ngModelChange)="selectedBillingAddress.expirationDate = $event"
            placeholder="mm/dd/yyyy"
            #dp8="ngbDatepicker" ngbDatepicker />
        <i class="cal-icon far fa-calendar" (click)="dp8.toggle()"></i>
    </div>
    <div style="color:red" *ngIf="expirationDate.invalid && selectedBillingAddress.expirationDate && (expirationDate.dirty || expirationDate.touched)">
        Invalid date.
    </div>
</div>
