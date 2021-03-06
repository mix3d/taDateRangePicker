# abDateRangePicker

### Pure AngularJS DateRangePicker (no jQuery required)

![alt tag](pure-angular-date-range-picker.png)

In looking for a datepicker for an older AngularJS, I came across this AngularJS library of a jQuery-less translation of the bootstrap date-range picker I had my eye on. I renamed it abDateRangePicker for **A**ngular-**B**ootstrap, after making substantial edits.

The original AngularJS code is adapted from [tawani's taDateRangePicker](https://github.com/tawani/taDateRangePicker), with the CSS style adapted from an older version of [dangrossman's bootstrap-daterangepicker](https://github.com/dangrossman/bootstrap-daterangepicker). I have since updated it to match that repo's newer styling.

The original project was rather dated, so I forked to upgrade to a more modern workflow and process, and put my own API preferences & interactions on the tool.

Feel free to fork and PR for the following features:

#### Goals:

- Configurable text-icon package
- Configurable Date Validation
  - non-selectable dates & date ranges
- More UI positioning logic, mobile responsive design.
  - templateable, like bootstrap-daterangepicker?
- ~~Configurable selection styles~~
  - Select predefined range = applies selection
  - Select predefined range = updates calendars selection, and always requires Apply button
- ~~Remove dependencies~~
  - ~~bindonce~~ _- done_
  - ~~moment-range~~ _- done_
    - Forked code didn't actually use any moment-range-specific things, just start / end and clone; replaced with POJO's.
- ~~Toggleable calendar icon display~~
- ~~Configurable date display formatting~~
- Upgrade to latest bootstrap-daterangepicker designs / dom
  - done, ish. Enough for now, at least!
- Proper Build Steps
  - ~~NPM config~~
  - ~~Sass~~
  - JS Compiler w/ Webpack/Babel

#### Nice to have:

- Locale & language configurable
  - Also, Configurable start-of-week display (Sunday vs Monday, other locales?)
  - *likely not going to happen, I don't have experience or a need for my US-only project*

## Usage:
### Add required files

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" />
    <link href="dist/ab-date-range-picker.css" rel="stylesheet" />

    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.11/angular.min.js"></script>
    <script type="text/javascript" src="libs/moment.min.js"></script>
    <script src="dist/ab-date-range-picker.js"></script>

### Sample usage

    <ab-date-range-picker ng-model="dateRange" ranges="customRanges"
            callback="dateRangeChanged()"></ab-date-range-picker>

### Sample Code

    angular.module("app", ['abDateRangePicker'])
        .controller("MainCtrl", ['$scope', function ($scope) {

            // specify default date range in controller
            $scope.dateRange = {
                start: moment("2015-12-05"),
                end: moment("2016-01-25")
            };

            //Select range options
            $scope.customRanges = [
                {
                    label: "This week",
                    range: {
                        start: moment().startOf("week").startOf("day"),
                        end: moment().endOf("week").startOf("day")
                    }
                },
                {
                    label: "Last month",
                    range: {
                        start: moment().add(-1, "month").startOf("month").startOf("day"),
                        end: moment().add(-1, "month").endOf("month").startOf("day")
                    }
                },
                {
                    label: "This month",
                    range: {
                        start: moment().startOf("month").startOf("day"),
                        end: moment().endOf("month").startOf("day")
                    }
                }
            ];

            $scope.dateRangeChanged = function() {
                console.log("the range was changed!");
            }

        }]);

### Dependencies

- [Moment.js](https://github.com/moment/moment)
  - currently assumes moment is loaded globally.

### Attributes

| Name                    | Type               | Default                              | Description                                                                                                                                                                                                                                          |
| ----------------------- | ------------------ | ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ng-model`              | Object, required   | `{ start: moment(), end: moment() }` | An object with start and end keys pointing to moment.js objects.                                                                                                                                                                                     |
| `ranges`                | Object, optional   |                                      | an array of objects, each with a label:"text" and range: {start, end} values. See above example.                                                                                                                                                     |
| `callback`              | Function, optional | `undefined`                          | Callback function is called when the dates are changed / applied.                                                                                                                                                                                    |
| `input-format`          | String, optional   | `"l"`                                | Format String for the display of the dates in the root picker button element, as <br>`start.format(input-format) - end.format(input-format)`<br>Defaults to `"l"`, for Locale specific formatting.                                                   |
| `picker-date-format`    | String, optional   | `"MM/DD/YYYY"`                       | Format String for the display of dates in the popup selector, above the calendar view.                                                                                                                                                               |
| `month-format`          | String, optional   | `"MMM YYYY"`                         | Format String for the display of the month and year above the calendar table.                                                                                                                                                                        |
| `always-show-calendars` | N/A                | `false`                              | Will always show the calendars. Default is to only show calendar if the custom range button is clicked.                                                                                                                                              |
| `must-apply`            | N/A                | `false`                              | Requires the Apply button to be clicked regardless of selecting a custom range or a predefined range. As a result, the selected date range will show on the calendar.<br>Ignores the `always-show-calendars` section and always shows the calendars. |
NNN
