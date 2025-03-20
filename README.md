if(this.oCurrentProviderLocationInfo.expirationDate && this.hasSingleBillingInfo(this.oCurrentProviderLocationInfo.serviceLocationId)){
      this.addService.fnRaiseWarningDlg("Not allowed","Provider must have at least one active billing location. If you are attempting to terminate this billing location, please add a new active billing location for this site prior to this action.");
      return;
     }

 public hasSingleBillingInfo(serviceLocationId){
    const serviceLocation = this.oProviderLocation.providerLocationInfo.filter(svc => svc.serviceLocationId === serviceLocationId)
    
    
    return serviceLocation[0].providerLocBillingInfo.length ===1
  }
