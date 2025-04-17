isTaxonomyFetched(npiIndex: number, taxonomyIndex: number): boolean {
  const npi = this.providerForm.providerAlternateIDSection?.[0]?.providerNpi?.[npiIndex];
  if (!npi) return false;

  const taxonomy = npi.providerTaxonomy?.[taxonomyIndex];
  return !!taxonomy?.id; // If taxonomy has `id`, it came from the API
}
