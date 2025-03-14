    <div class="form-group col-12" style="display: inline-flex;">
        <label for="locationExpirationDate">Expiration Date<span class="required-star"> *</span></label>
        <div class="input-group" style="max-width: 50% !important; margin-left: 3% !important;">
            <input maxlength="10" dateFormater class="data-input form-control form-control-custom"
                [ngClass]="{'red-border': (!sLocationExpirationDate && isTerminateBtnClicked)}"
                placeholder="mm/dd/yyyy" id="locationExpirationDate" name="locationExpirationDate"
                [(ngModel)]="sLocationExpirationDate" #dp4="ngbDatepicker" ngbDatepicker required />
            <i class="cal-icon far fa-calendar" (click)="dp4.toggle()"></i>
        </div>
        <div class="error-tooltip error-tooltip-offset" style="margin-top: 1% !important; margin-right: 25% !important;"
            *ngIf="!sLocationExpirationDate && isTerminateBtnClicked">
            Expiration Date is Required to terminate {{isBillingNotProvider ? 'billing' : 'practice'}} location
        </div>
    </div>
</div>
