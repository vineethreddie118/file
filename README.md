  public fnHandleNpiUpdatedEvent(data: NpiTaxonomyData) {
    // console.log(data);
    this.isNpiTaxonomyValid = data.bIsValid;
    if(this.providerForm.providerAlternateIDSection[0] !=undefined){
    this.providerForm.providerAlternateIDSection[0].providerNpi = [...data.providerAlternateIDSection[0].providerNpi];
  }
}

  public fnHandleNpiUpdatedEvent(data: NpiTaxonomyData) {
    // console.log(data);
    this.isNpiTaxonomyValid = data.bIsValid;
    if(data){
    this.providerForm.providerAlternateIDSection = data.providerAlternateIDSection;
  }
}
