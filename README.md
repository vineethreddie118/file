} else if (this.providerForm.providerAlternateIDSection?.[0]?.providerNpi?.[0]?.npi) {
  this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy =
    (this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy || []).filter(
      (taxObj) => taxObj.taxonomyCode
    );
