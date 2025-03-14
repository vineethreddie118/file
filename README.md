<div class="form-group col-md-6 col-lg-4 col-xl-3">
  <label for="effectiveDate">Effective Date<span class="required-star"> *</span></label>
  <div class="input-group">
    <input maxlength="10" dateFormater class="data-input form-control"
      [ngClass]="{'red-border': (effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched))}"
      placeholder="mm/dd/yyyy" name="effectiveDateupdate" #effectiveDate="ngModel"
      [(ngModel)]="selectedBillingAddress.effectiveDate"
      #dp1="ngbDatepicker" ngbDatepicker/>
    <i class="cal-icon far fa-calendar" (click)="dp1.toggle()"></i>
    <div class="error-tooltip">
      <ng-container *ngIf="effectiveDate.invalid && (effectiveDate.dirty || effectiveDate.touched)">Invalid date</ng-container>
    </div>
  </div>
</div>
