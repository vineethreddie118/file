<div class="form-group col-md-6 col-lg-4 col-xl-3">
    <label for="expirationDate">Expiration Date</label>
    <div class="input-group">
        <input type="text" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
            [ngClass]="{'red-border': (expirationDate.invalid && (expirationDate.dirty || expirationDate.touched))}"
            placeholder="mm/dd/yyyy" name="expirationDate" #expirationDate="ngModel"
            [ngModel]="oCurrentProviderLocationInfo.expirationDate | date: 'MM/dd/yyyy'"
            (ngModelChange)="oCurrentProviderLocationInfo.expirationDate = $event"
            #dp5="ngbDatepicker" ngbDatepicker />
        <i class="cal-icon far fa-calendar" (click)="dp5.toggle()"></i>
        <div class="error-tooltip" *ngIf="expirationDate.invalid && (expirationDate.dirty || expirationDate.touched)">
            Invalid date
        </div>
    </div>
</div>

