this.addService
  .fnRaiseGenericAlert('saveProfileInformation', err)
  .result.then((closeResult) => {
    const nUPDId = this.addService.fnIsUPDInsertSuccess(err);
    
    if (nUPDId !== 0) {
      // If UPD insert success returns a valid provider ID, update it and proceed
      this.providerForm.providerId = nUPDId;
      this.updateProviderIdEvent.next(this.providerForm);

      this.navigationEvent.next({
        enumCurrentScreen: ScreenType.PROFILE,
        bIsNext: true,
      });
    } else {
      // If provider creation failed in UPD and still 0, prevent navigation
      this.addService.fnRaiseGenericAlert(
        'Provider Creation failed in UPD.',
        err
      );
      return;  // Stop execution and prevent moving to the next screen
    }
  });
