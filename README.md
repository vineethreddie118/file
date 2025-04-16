const npiValue = this.providerForm.providerAlternateIDSection?.[0]?.providerNpi?.[0]?.npi;

if (this.providerForm.providerTypeId === ProviderTypeEnum.GROUP && !npiValue) {
  this.providerForm.providerAlternateIDSection = null;
} else {
  // retain NPI with empty taxonomy if present
  this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy =
    this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy.filter(
      (taxObj) => taxObj.taxonomyCode
    );
  const requestTaxnomy = [];
  for (const taxonomy of this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy) {
    const requestTaxonomy = {
      id: this.getTaxonamtByCode(taxonomy),
      taxonomyCode: taxonomy?.taxonomyCode,
      isPrimary: taxonomy.isPrimary ? true : false,
      active: taxonomy?.active === true ? true : false,
      isDelete: false,
      effectiveDate: taxonomy.effectiveDate,
      expirationDate: taxonomy.expirationDate,
      providerNPIID: taxonomy.providerNPIID
    };
    requestTaxnomy.push(requestTaxonomy);
  }
  this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy = requestTaxnomy;
}
