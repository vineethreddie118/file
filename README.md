<div class="form-group col-12" style="display:inline-flex">
        <label for="locationExpirationDate">Expiration Date<span class="required-star"> *</span></label>
        <input style="max-width:50% !important; margin-left: 3% !important" type="date" class="form-control form-control-custom"
          id="locationExpirationDate" name="locationExpirationDate" [(ngModel)]="sLocationExpirationDate" required />
        <div style="display:inline-flex; margin-top:1% !important; margin-right:25% !important"
          class="error-tooltip error-tooltip-offset" *ngIf="!sLocationExpirationDate && isTerminateBtnClicked">
          Expiration Date is Required to terminate {{isBillingNotProvider ? 'billing' : 'practice'}} location
        </div>
      </div>
