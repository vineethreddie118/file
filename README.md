 this.oLocationToView.providerSvcLocHours.sort((a, b) => {
            const dayDiff = days.indexOf(a.dayOfWeek) - days.indexOf(b.dayOfWeek);
            if (dayDiff !== 0) return dayDiff;
            return a.openTimeValue.localeCompare(b.openTimeValue);
        });
