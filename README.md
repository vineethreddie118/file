<div class="form-group col-md-6 col-lg-4 col-xl-3">
    <label for="expirationDate">Expiration Date</label>
    <div class="input-group">
        <input type="text" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
            [ngClass]="{'red-border': (expirationDateNpi.invalid && (expirationDateNpi.dirty || expirationDateNpi.touched))}"
            placeholder="mm/dd/yyyy" name="expirationDate{{npiindex}}" #expirationDateNpi="ngModel"
            [ngModel]="oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[npiindex].expirationDate | date: 'MM/dd/yyyy'"
            (ngModelChange)="oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[npiindex].expirationDate = $event"
            #dp3="ngbDatepicker" ngbDatepicker />
        <i class="cal-icon far fa-calendar" (click)="dp3.toggle()"></i>
        <div class="error-tooltip">
            <ng-container *ngIf="expirationDateNpi.invalid && (expirationDateNpi.dirty || expirationDateNpi.touched)">Invalid date</ng-container>
        </div>
    </div>
</div>
