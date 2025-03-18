 // Check if provider ID is 0 and error status is 500
  if (this.providerForm.providerId === 0 && err?.status === 500) {
    const errorMessage = err?.error?.errors?.join(' ') || 'An unexpected error occurred.';
    
    // Show the exact backend error message
    this.addService.fnRaiseGenericAlert(
      'Error',
      errorMessage
    );
    return; // Prevent navigation
  }
