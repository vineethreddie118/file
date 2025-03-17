public fnHandleReactivateClick(): void {
    if (this.oLocationToReactivate.newEffectiveDate &&
      new Date(this.oLocationToReactivate.newEffectiveDate) > new Date(this.oLocationToReactivate.oldExpirationDate)) {
      this.oLocationToReactivate.bIsValidEffectiveDate = true;
      this.oReactiavteModalRef.close();
      const payload = {
        providerId: this.nProviderId,
        userId: this.oSearchformSvc.userDetail?.userID || 2,
        providerLocBillingInfo : [
          {
            providerSvcLocPayeeId: this.oLocationToReactivate?.oLocationObject?.providerSvcLocPayeeId,
            providerSvcLocPayeeMapID: this.oLocationToReactivate?.oLocationObject?.providerSvcLocPayeeMapID,
            serviceLocationId: this.oLocationToReactivate?.oLocationObject?.serviceLocationId,
            effectiveDate: new Date(this.oLocationToReactivate?.newEffectiveDate)?.toISOString(),
            expirationDate: null
          }
        ]
      };
      this.bIsLoading = true;
      this.searchService.reactivateBillingLocation(payload).subscribe((d) => {
        this.fnSetInactiveBillingLocationSectionVisibility(false);
        setTimeout(() => {
          this.fnFetchData();
          this.bIsLoading = false;
        }, 0);
      })
      
    }
    else {
      this.oLocationToReactivate.bIsValidEffectiveDate = false;
    }
    this.oLocationToReactivate.bIsReactivateClicked = true;
  }
