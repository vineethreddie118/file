<div class="form-group col-md-6 col-lg-4 col-xl-3">
    <label for="effectiveDate">Effective Date</label>
    <div class="input-group">
        <input type="text" maxlength="10" dateFormater class="form-control form-control-custom custom-date-input"
            [ngClass]="{'red-border': (effectiveDateNpi.invalid && (effectiveDateNpi.dirty || effectiveDateNpi.touched))}"
            placeholder="mm/dd/yyyy" name="effectiveDate{{npiindex}}" #effectiveDateNpi="ngModel"
            [ngModel]="oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[npiindex].effectiveDate | date: 'MM/dd/yyyy'"
            (ngModelChange)="oCurrentProviderLocationInfo.providerAlternateIDSection[0].providerNpi[npiindex].effectiveDate = $event"
            #dp2="ngbDatepicker" ngbDatepicker />
        <i class="cal-icon far fa-calendar" (click)="dp2.toggle()"></i>
        <div class="error-tooltip">
            <ng-container *ngIf="effectiveDateNpi.invalid && (effectiveDateNpi.dirty || effectiveDateNpi.touched)">Invalid date</ng-container>
        </div>
    </div>
</div>
