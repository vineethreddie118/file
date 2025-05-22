public fnHasAtLeastOneActiveBilling(location: any): boolean {
  if (!location?.providerLocBillingInfo || location.providerLocBillingInfo.length === 0) {
    return false; // No billing info => treat as no active billing
  }

  // Return true if at least one billing is active
  return location.providerLocBillingInfo.some(billing => billing.isblActive === true);
}
