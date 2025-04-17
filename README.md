<div class="col-md-6 col-lg-4 col-xl-3">
            <label for="inputData">Taxonomy Code</label>
            <select class="form-control form-control-custom" id="Taxonomy{{idx}}{{i}}" name="Taxonomy{{idx}}{{i}}"
                    #Taxonomy="ngModel" [(ngModel)]="taxonomy.taxonomyCode" (change)="validateForPrimaryExist()">
              <option value=""></option>
              <ng-container *ngFor="let providerType of taxonomyCodes">
                <option [ngValue]="providerType.taxonomyCode">
                  {{providerType.taxonomyCode}}
                </option>
              </ng-container>
            </select>
          </div>

private fnFetchData(): void {
    let arrCallsToMake = [
      null === this.providerLookupDetails
        ? this.searchService.getProviderProfileLookup().pipe(
          catchError((err) => {
            this.bIsLoading = false;
            this.addService.fnRaiseGenericAlert('getLookupDetails');
            return of(null);
          })
        )
        : of(null),
      this.providerForm.providerId !== undefined
        ? this.searchService
          .getProfileInformation(this.providerForm.providerId)
          .pipe(
            catchError((err) => {
              this.bIsLoading = false;
              this.addService.fnRaiseGenericAlert('getProfileInformation');
              return of(null);
            })
          )
        : of(null),
    ];

    this.bIsLoading = true;
    forkJoin([...arrCallsToMake])
      .pipe(
        map(([lookupInfo, providerInfo]) => {
          if (!!lookupInfo) {
            this.providerLookupDetails = { ...lookupInfo[0] };
          }

          if (!!providerInfo) {
            this.providerForm = { ...providerInfo };
            this.providerForm.providerId = this.nProviderId;
            this.bIsLocationValidated = this.providerForm['addressValidated'];
            this.savetoFlexcare = this.providerForm['savetoFlexcare'];
            // Apply address trimming logic
            // if (!!this.providerForm.address1 && this.providerForm.address1.length > 25) {
            //   this.providerForm.address2 = this.providerForm.address1.substring(25) + `${this.providerForm.address2 ? ', ' + this.providerForm.address2 : ''}`;
            //   this.providerForm.address1 = this.providerForm.address1.substring(0, 25);
            // }
            if (this.providerForm.phone) {
              this.providerForm.phone = this.formatNumbersonfetch(this.providerForm.phone);
            }
            if (this.providerForm.fax) {
              this.providerForm.fax = this.formatNumbersonfetch(this.providerForm.fax);
            }
            if (this.providerForm.mobile) {
              this.providerForm.mobile = this.formatNumbersonfetch(this.providerForm.mobile);
            }

            if (
              !!this.providerForm.providerAlternateIDSection &&
              this.providerForm.providerAlternateIDSection.length > 0
            ) {
              this.providerForm.providerAlternateIDSection[0].medicare =
                new Medicare();
              if (
                this.providerForm.providerAlternateIDSection[0].providerNpi[0]
                  .providerTaxonomy &&
                this.providerForm.providerAlternateIDSection[0].providerNpi[0]
                  .providerTaxonomy.length === 0
              ) {
                this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy =
                  [];
              } else  {
                this.providerForm.providerAlternateIDSection[0].providerNpi[0].providerTaxonomy.forEach(
                  (taxonamy, i) => {
                    if (taxonamy.taxonomyCode.length) {
                      this.disabledTaxonomies.push(taxonamy.taxonomyCode);
                    }
                  }
                );
              }
            }
            this.defaultProviderForm = JSON.parse(
              JSON.stringify(this.providerForm)
            );
            this.fnSetAddress();
          }
          this.bIsLoading = false;
        })
      )
      .subscribe();
  }

{
    "caqhid": null,
    "addressValidated": true,
    "npi": "5645645756",
    "primaryLicenseCodeID": null,
    "secondaryLicenseCodeID": null,
    "primaryLicenseCodeDescription": null,
    "secondaryLicenseCodeDescription": null,
    "mbhpid": null,
    "status": false,
    "casid": null,
    "providerContactName": null,
    "providerContactTypeID": null,
    "delegatedAgencyFlag": null,
    "flexcareID": "378601",
    "socialSecurity": "",
    "lastName": "",
    "firstName": "",
    "middleName": "",
    "name": "HFGH",
    "dob": null,
    "genderId": null,
    "genderName": "",
    "titleId": null,
    "titleName": "",
    "modifierId": null,
    "modifier": "",
    "caqhAttestationDate": null,
    "organizationName": "hfgh",
    "taxId": "575675676",
    "raceId": null,
    "raceName": null,
    "ethnicityId": null,
    "ethnicityName": null,
    "prefixId": 1,
    "prefixDesc": "1-9",
    "providerAlternateIDSection": [
        {
            "providerNpi": [
                {
                    "providerSvcLocNpiId": 0,
                    "npi": "4345435435",
                    "effectiveDate": "04/01/2025",
                    "expirationDate": "04/30/2025",
                    "providerNPIID": 782545,
                    "serviceLocationId": 0,
                    "providerTaxonomy": [
                        {
                            "id": 18,
                            "taxonomyCode": "103GC0700X",
                            "isPrimary": true,
                            "active": false,
                            "isDelete": false,
                            "effectiveDate": "04/04/2025",
                            "expirationDate": "04/22/2025",
                            "providerNPIID": 782545
                        }
                    ],
                    "isDelete": false
                },
                {
                    "providerSvcLocNpiId": 0,
                    "npi": "5645645756",
                    "effectiveDate": "04/03/2025",
                    "expirationDate": "04/26/2025",
                    "providerNPIID": 782546,
                    "serviceLocationId": 0,
                    "providerTaxonomy": [
                        {
                            "id": 21,
                            "taxonomyCode": "103TA0400X",
                            "isPrimary": true,
                            "active": false,
                            "isDelete": false,
                            "effectiveDate": "04/17/2025",
                            "expirationDate": "04/25/2025",
                            "providerNPIID": 782546
                        }
                    ],
                    "isDelete": false
                }
            ],
            "medicare": {}
        }
    ],
    "groupAffiliation": null,
    "groupPractitioners": null,
    "practitionarsCount": 0,
    "hasModifiedProfileInfo": true,
    "hasModifiedProviderNpi": true,
    "hasModifiedProviderTaxonomy": true,
    "email": null,
    "phone": null,
    "phoneExt": null,
    "fax": null,
    "website": null,
    "mobile": null,
    "providerTypeId": 2,
    "providerType": "GROUP",
    "providerStatusId": 1,
    "providerStatus": "ACTIVE",
    "addressId": 1378999,
    "address1": "67 1/2 2nd St",
    "address2": "",
    "city": "Ilion",
    "stateCode": "NY",
    "county": "Herkimer",
    "zip": "13357-2117",
    "providerId": 1133091,
    "userId": 2,
    "errors": null,
    "savetoFlexcare": true
}
