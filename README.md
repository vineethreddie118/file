const npiObj = this.providerForm.providerAlternateIDSection?.[0]?.providerNpi?.[0];

if (
  this.providerForm.providerTypeId === ProviderTypeEnum.GROUP ||
  (!npiObj?.npi && (!npiObj?.providerTaxonomy || npiObj.providerTaxonomy.length === 0))
) {
  this.providerForm.providerAlternateIDSection = null;
} else {
  // If taxonomy exists, filter and map
  if (npiObj?.providerTaxonomy?.length) {
    npiObj.providerTaxonomy = npiObj.providerTaxonomy.filter((taxObj) => taxObj.taxonomyCode);

    for (const taxonomy of npiObj.providerTaxonomy) {
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

    npiObj.providerTaxonomy = requestTaxnomy;
  } else {
    // Taxonomy not entered, keep providerTaxonomy as empty array
    npiObj.providerTaxonomy = [];
  }
}
