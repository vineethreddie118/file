<div class="form-group col-md-6 col-lg-4 col-xl-3">
    <label for="expirationDate">Expiration Date</label>
    <div class="input-group">
        <input maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
            [ngClass]="{'red-border': ( expirationDate.invalid && (expirationDate.dirty || expirationDate.touched|| formsubmitted ))}"
            placeholder="mm/dd/yyyy" name="expirationDate" #expirationDate="ngModel"
            [(ngModel)]="oCurrentProviderLocationInfo.expirationDate"
            #dp2="ngbDatepicker" ngbDatepicker />
        <i class="cal-icon far fa-calendar" (click)="dp2.toggle()"></i>
        <div class="error-tooltip">
            <ng-container *ngIf="expirationDate.invalid && (expirationDate.dirty || expirationDate.touched)">Invalid date</ng-container>
        </div>
    </div>
</div>
