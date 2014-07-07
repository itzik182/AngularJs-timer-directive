AngularJs timer directive
=========================
hi this is a demo:

http://plnkr.co/edit/qN7y0QnM6iTCDg8az5wg?p=preview

code:

var myApp = angular.module('myApp', []); 

angular.module('myApp').directive('itzikTimer', ['$interval', function ($interval) {
        return {
            restrict: 'E',
            //require: 'ngModel',
            scope: {
                started: '=',
                reset: '=',
                ngModel: '='
            },
            templateUrl: 'timer.html',
            replace: true,
            link: function (scope, element, attrs, model) {
                //if(!intervalObj)
                    var intervalObj;
                scope.$watch('started', function (newVal) {
                 
                    if (newVal == true) {
                        intervalObj = $interval(function () {
                            scope.ngModel.add('seconds', 1);
                            
                        }, 1000);
                    }
                    else {
                        $interval.cancel(intervalObj);
                        //scope.$on('$destroy', function () { $interval.cancel(intervalObj); });
                    }
                     //alert(intervalObj);
                });
                scope.$watch('reset', function (newVal) {
                    if (newVal == true) {
                      scope.reset = false;
                        scope.ngModel = moment().minutes(0).seconds(0).hours(0).milliseconds(0);
                    }
                });
                scope.$watch('ngModel', function (newVal) {
                    scope.ngModel = newVal;
                });
            }
        }
    }]);
    
    myApp.filter('moment', [function () {
        return function (dateString, format) {
            return moment.utc(dateString).format(format);
        };
    }]);

myApp.controller('serviceCtrl', ["$scope", function ($scope) {
  $scope.timeElapsed = moment().minutes(0).seconds(0);
  
  $scope.startTimer = function(){
    $scope.startTimer = true;
  };
  $scope.stopTimer = function(){
    $scope.startTimer = false;
  };
  $scope.resetTimer = function(){
    $scope.resetTimer = true;
  };
}]);

html:

<!DOCTYPE html>
<html ng-app="myApp">

  <head>
    <script data-require="angular.js@*" data-semver="1.3.0-beta.5" src="https://code.angularjs.org/1.3.0-beta.5/angular.js"></script>
    <script data-require="jquery@*" data-semver="2.1.1" src="//cdnjs.cloudflare.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
    <script data-require="moment.js@*" data-semver="2.5.1" src="//cdnjs.cloudflare.com/ajax/libs/moment.js/2.5.1/moment.js"></script>
    <link rel="stylesheet" href="style.css" />
    <script src="script.js"></script>
  </head>

  <body ng-controller="serviceCtrl">
    <h1>AngularJs timer directive</h1>
    <div>
      <itzik-timer ng-model="timeElapsed" started="startTimer" reset="resetTimer"></itzik-timer>    
    </div>
    <button type="button" data-ng-click="startTimer()">
        Start
    </button>
    
    <button type="button" data-ng-click="stopTimer()">
        Stop
    </button>
                    
    <button type="button" data-ng-click="resetTimer()">
        Reset
    </button>
  </body>

</html>
