<div class="col-md-6 col-lg-4 col-xl-3">
    <label for="locationExpirationDate">Expiration Date<span class="required-star"> *</span></label>
    <div class="input-group">
        <input type="date" class="data-input form-control" 
            [ngClass]="{'red-border': (!sLocationExpirationDate && isTerminateBtnClicked)}"
            id="locationExpirationDate" name="locationExpirationDate" 
            [(ngModel)]="sLocationExpirationDate" required />
        <i class="cal-icon far fa-calendar" (click)="dpExpDate.toggle()"></i>
        <div class="error-tooltip">
            <ng-container *ngIf="!sLocationExpirationDate && isTerminateBtnClicked">
                Expiration Date is Required to terminate {{isBillingNotProvider ? 'billing' : 'practice'}} location
            </ng-container>
        </div>
    </div>
</div>
