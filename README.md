public fnAllBillingLocationsInactive(location: any): boolean {
  if (!location?.providerLocBillingInfo?.length) return false;
  return location.providerLocBillingInfo.every(billing => billing.isblActive === false);
}
