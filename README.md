<table class="table table-bordered">
                      <thead>
                        <th>Day of week</th>
                        <th>Open time</th>
                        <th>Close time</th>
                      </thead>
                      <tbody *ngIf="oLocationToView.providerSvcLocHours?.length >0">
                        <tr *ngFor="let officeHour of oLocationToView.providerSvcLocHours; let i = index">
                          <td>{{officeHour.dayOfWeek}}</td>
                          <td><input type="time" style="border: none;" value="{{officeHour.openTimeValue}}" readonly></td>
                          <td><input type="time" style="border: none;" value="{{officeHour.closeTimeValue}}" readonly></td>
                        </tr>
                      </tbody>
                    </table>

public fnViewLocation(oLocation: any, index: number, bIsInactiveLocation: boolean): void {
    this.bViewLocation = true;
    this.oLocationToView = null;
    this.isViewPracticeLocationAccordionOpen = true;
    this.oLocationToView = JSON.parse(JSON.stringify(oLocation));
    this.fnAdjustOfficeHours();

    this.bIsClickFromInactivePracLocation = bIsInactiveLocation;

    setTimeout(() => {
      this.addService.scrollToTarget(this.viewLocationSectionElem);
    }, 0);
    
  }

  private fnAdjustOfficeHours(): void {
    const days = ['MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT', 'SUN'];
    if(!this.oLocationToView?.providerSvcLocHours) {
      this.oLocationToView.providerSvcLocHours = [];
      days.forEach((day) => {
        this.fnInsertDefaultOfficeHourFor(day)
      })
    }
    else {
      for (let i = 0; i < days.length; i++) {
        const isFound = this.oLocationToView.providerSvcLocHours.find((oOfficeHrAt) => oOfficeHrAt.dayOfWeek === days[i]);
        if (!isFound) {
          this.fnInsertDefaultOfficeHourFor(days[i]);
        }
      }
      this.oLocationToView.providerSvcLocHours.sort((a,b) => days.indexOf(a.dayOfWeek) - days.indexOf(b.dayOfWeek));
    }
  }

  private fnInsertDefaultOfficeHourFor(dayOfWeek) {
    
    this.oLocationToView.providerSvcLocHours.push({dayOfWeek: dayOfWeek, openTimeValue: null, closeTimeValue: null});
  }
