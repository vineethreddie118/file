<ng-container *ngIf="!location.isslActive || (location.isslActive && location.providerLocBillingInfo?.length > 0 && location.providerLocBillingInfo.filter(b => b.isblActive).length === 0)">
  <div class="text-center d-flex justify-content-center table-btn-section">
    <i class="fa-solid fa-regular fa-eye btn btn-primaryicon btn-sm" style="margin-right: 6px;"
       (click)="fnViewLocation(location, i, true)" title="View Location"></i>
  </div>
</ng-container>
