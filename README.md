<div class="col-md-6 col-lg-4 col-xl-3">
    <label for="locationExpirationDate">Expiration Date<span class="required-star"> *</span></label>
    <div class="input-group">
        <input maxlength="10" dateFormater class="data-input form-control"
            [ngClass]="{'red-border': (!sLocationExpirationDate && isTerminateBtnClicked)}"
            placeholder="mm/dd/yyyy" name="locationExpirationDate" #dc4="ngModel"
            [(ngModel)]="sLocationExpirationDate" 
            #dp4="ngbDatepicker" ngbDatepicker required />
        <i class="cal-icon far fa-calendar" (click)="dp4.toggle()"></i>
        <div class="error-tooltip">
            <ng-container *ngIf="!sLocationExpirationDate && isTerminateBtnClicked">
                Expiration Date is Required to terminate {{isBillingNotProvider ? 'billing' : 'practice'}} location
            </ng-container>
        </div>
    </div>
</div>
