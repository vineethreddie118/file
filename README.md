<ng-container *ngIf="!location.isslActive || (location.isslActive && location.providerLocBillingInfo?.length > 0 && location.providerLocBillingInfo.filter(b => b.isblActive).length === 0)">
