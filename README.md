<ng-container *ngIf="location.isslActive ">
                  <div class="text-center d-flex justify-content-center table-btn-section">
                    <i class="fa-solid fa-regular fa-eye btn btn-primaryicon  btn-sm " style="margin-right: 6px;"
                      (click)="fnViewLocation(location, i, false)" title="View Location"></i>
                    <i class="fa-solid fa-pen-to-square btn btn-primaryicon  btn-sm " style="margin-right: 6px;"
                      data-toggle="tooltip" (click)="fnEditLocation(i)" title="Edit Location"></i>
                    <i class="fa-solid fa fa-ban btn btn-primaryicon  btn-sm " data-toggle="tooltip" style="margin-right: 6px;"
                      title="Terminate" (click)="fnTerminateLocation(i,modalDeleteLocation, location,false)"></i>
                    <i class="fa-solid fa fa-exchange btn btn-primaryicon btn-sm" data-toggle="tooltip"
                      title="Replace Practice Location" (click)="fnReplaceLocation(location,i)"></i>
                  </div>
                </ng-container>
