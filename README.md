<div class="form-group" style="display:inline-flex; margin-top: 12px;">
        <label for="newEffectiveDate">Effective Date<span class="required-star"> *</span></label>
        <input style="max-width:50% !important; margin-left: 3% !important" type="date" class="form-control form-control-custom"
          id="newEffectiveDate" name="newEffectiveDate" [(ngModel)]="oLocationToReactivate.newEffectiveDate" required />
      </div>
      <div *ngIf="false === oLocationToReactivate?.bIsValidEffectiveDate && oLocationToReactivate?.bIsReactivateClicked"
        style="color: #ff0000;">
        The reactivate effective date is required and cannot be on or earlier than the previous expiration date ({{oLocationToReactivate.oldExpirationDate}}).
      </div>
