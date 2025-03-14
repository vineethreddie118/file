<div class="form-group col-md-6 col-lg-4 col-xl-3">
  <label for="effectiveDate">Effective Date<span class="required-star"> *</span></label>
  <div class="input-group">
    <input maxlength="10" dateFormater class="data-input form-control"
      [ngClass]="{'red-border': (effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched || formsubmitted))}"
      placeholder="mm/dd/yyyy" name="effectiveDate" #effectiveDate="ngModel"
      [(ngModel)]="oCurrentProviderLocationInfo.effectiveDate"
      #dp3="ngbDatepicker" ngbDatepicker/>
    <i class="cal-icon far fa-calendar" (click)="dp3.toggle()"></i>
    <div class="error-tooltip">
      <ng-container *ngIf="effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched || formsubmitted)">
        Required
      </ng-container>
    </div>
  </div>
</div>
