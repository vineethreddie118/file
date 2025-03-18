if (err.status === 500 && err.errors && err.errors.length > 0) {
    console.log('Setting bIsLoading to false before error message');
    this.bIsLoading = false;
    
    const backendErrorMessage = err.errors.join('\n');
    this.addService.fnRaiseWarningDlg('Error', backendErrorMessage);
    
    return; // Ensure no further execution
}
