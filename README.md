{
    "providerLocationInfo": [
        {
            "locationName": "NORVY MINDFUL CARE PSYCHIATRY NP PC",
            "phone": "5512755674",
            "phoneExt": null,
            "email": null,
            "fax": null,
            "isPrimary": true,
            "locationAltId": null,
            "addressId": 1278742,
            "serviceAddress": "1400 Wantagh Ave Ste 202",
            "address2": "",
            "city": "Wantagh",
            "defaultCity": "Wantagh",
            "stateCode": "NY",
            "zip": "11793-2210",
            "country": "US",
            "county": "Nassau",
            "serviceLocationId": 11273,
            "providerSvcLocTypeId": 2,
            "publicTransportationFlag": false,
            "handicappedAccessibleFlag": false,
            "providerServiceLocationMapId": 60917,
            "addressValidated": true,
            "userId": 2,
            "providerSvcLocHours": null,
            "providerAlternateIDSection": [
                {
                    "providerNpi": [
                        {
                            "providerSvcLocNpiId": 21155,
                            "npi": "1265187249",
                            "effectiveDate": "1001-12-31T00:00:00",
                            "expirationDate": null,
                            "serviceLocationId": 11273,
                            "providerTaxonomy": [
                                {
                                    "id": 136,
                                    "taxonomyCode": "193400000X",
                                    "isPrimary": false,
                                    "active": true,
                                    "isDelete": false
                                }
                            ],
                            "isDelete": false
                        }
                    ]
                }
            ],
            "providerLocBillingInfo": [
                {
                    "providerSvcLocPayeeId": 14580,
                    "providerSvcLocPayeeMapID": 153353,
                    "serviceLocationId": 0,
                    "taxId": "874318665",
                    "taxTypeId": "E",
                    "w9flag": true,
                    "payeeName": "NORVY MINDFUL CARE PSYCHIATRY NP PC",
                    "billingAddressId": 1278742,
                    "billingAddress1": "1400 Wantagh Ave Ste 202",
                    "billingAddress2": "",
                    "billingCity": "Wantagh",
                    "billingStateCode": "NY",
                    "billingZip": "11793-2210",
                    "billingCounty": "Nassau",
                    "billingDefaultCity": "Wantagh",
                    "billingCountry": "US",
                    "billingAddressValidated": true,
                    "effectiveDate": "2023-08-07T00:00:00",
                    "expirationDate": null,
                    "isDelete": false,
                    "isReplace": false
                },
                {
                    "providerSvcLocPayeeId": 14580,
                    "providerSvcLocPayeeMapID": 153354,
                    "serviceLocationId": 0,
                    "taxId": "874318665",
                    "taxTypeId": "E",
                    "w9flag": true,
                    "payeeName": "NORVY MINDFUL CARE PSYCHIATRY NP PC",
                    "billingAddressId": 1278742,
                    "billingAddress1": "1400 Wantagh Ave Ste 202",
                    "billingAddress2": "",
                    "billingCity": "Wantagh",
                    "billingStateCode": "NY",
                    "billingZip": "11793-2210",
                    "billingCounty": "Nassau",
                    "billingDefaultCity": "Wantagh",
                    "billingCountry": "US",
                    "billingAddressValidated": true,
                    "effectiveDate": "2023-08-07T00:00:00",
                    "expirationDate": null,
                    "isDelete": false,
                    "isReplace": false
                }
            ],
            "isDelete": false,
            "replaceLocId": "",
            "isGrpServiceLocation": true,
            "grpPractitionersCount": 1,
            "groupInfo": [
                {
                    "providerId": 29589,
                    "serviceLocationId": 11273,
                    "providerServiceLocationMapId": 60917
                }
            ]
        },
        
    ],
    "providerId": 29589,
    "userId": 2,
    "errors": null
}

<div *ngIf="oCurrentProviderLocationInfo?.isGrpServiceLocation" class="warningMsg warningText">This Tax ID belongs to an organization. You need to edit this address in the organization rather from here.</div>
                <form class="form-row">
                  <fieldset [disabled]="oCurrentProviderLocationInfo?.isGrpServiceLocation">
