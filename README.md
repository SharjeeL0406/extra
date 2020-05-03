<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AngularJS</title>

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/limonte-sweetalert2/8.11.8/sweetalert2.css">
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.9/angular.min.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <link rel="icon" href="angular.png" type="image/png" sizes="16x16">
    <script src="angularjs.js"></script>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/limonte-sweetalert2/8.11.8/sweetalert2.all.js"></script>

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
    <script>
	 var myapp = angular.module("myApp", []);
	myapp.controller('myController', function($scope, $http) {
        URL = "http://labapi.somee.com/api/Course";

        //DISPLAY
        $http.get(URL).then(function (result) {
        $scope.courseList = result.data;
        });



        //ADDING NEW DATA
        $scope.addCourse = function ()
    {
        var jsonObj = {"courseName": $scope.course_Name, "courseCode": $scope.course_Code};

        //$http POST function
        $http.post(URL, jsonObj).then(function (result) {
        Swal.fire(
            'Good job!',
            'Record Added Successfully!',
            'success'
            );
            $http.get(URL).then(function (result) {
                $scope.courseList = result.data;
            });
        });


    }


        //Used to fetch Data on Modal Dialog
        $scope.editCourse = function (id)
    {
        console.log(id);
        editURL = "http://labapi.somee.com/api/Course/" + id;
        $http.get(editURL).then(function (result)
        {
        course = result.data;
            $scope.course_name = course.courseName;
            $scope.course_code = course.courseCode;

        });
    }



    //Update Course
    $scope.saveChanges = function () {
        console.log("Course ID: " + course.Id);
        var updateURL =  "http://labapi.somee.com/api/Course/" + course.Id;
        var cobj = {"courseName": $scope.course_name, "courseCode": $scope.course_code};
        console.log(cobj);

        //$http PUT function
        $http.put(updateURL, cobj).then(function successCallback(response) {


        Swal.fire(
            'Good job!',
            'Record Updated Successfully!',
            'success'
        )

    }, function errorCallback(response) {
        console.log(response);

                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: 'Something went wrong!',
                    footer: 'Error, is your internet working?'
                })

                $http.get(URL).then(function (result) {
                    $scope.courseList = result.data;
                });

        });

    };



    //Delete Course
        $scope.deleteCourse = function (ccourse) {
        url = "http://labapi.somee.com/api/Course/" + ccourse.Id

        //$http DELETE function
        $http.delete(url).then(function successCallback(response) {


        Swal.fire(
            'Good job!',
            'Record Deleted Successfully!',
            'success'
        )
            var index = $scope.courseList.indexOf(ccourse);
            $scope.courseList.splice(index, 1);

        }, function errorCallback(response) {

                Swal.fire({
                    icon: 'error',
                    title: 'Oops...',
                    text: 'Something went wrong!',
                    footer: 'Error, is your internet working?'
                })
                $http.get(URL).then(function (result) {
                    $scope.courseList = result.data;
                });

        });

    };




});

    </script>

</head>
<body>
    <br />
    <div class="container" ng-app="myApp" ng-controller="myController">
        <form>
            <h1 style="color:orangered">COURSE DATA</h1>
            <div>

                <div class="form-group">
                    <label for="courseName">Course Name:</label>
                    <input type="text" class="form-control" ng-model="course_Name" placeholder="Course Name">
                </div>
                <div class="form-group">
                    <label for="CNIC">Course Code:</label>
                    <input type="text" class="form-control" ng-model="course_Code" placeholder="Course Code">
                </div>

                <button ng-click="addCourse()" type="button" id="btnSubmit" class="btn btn-danger">Save Record</button>
            </div>
        </form>
        <br />

        <table class="table table-hover">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Course Name</th>
                    <th>Course Code</th>
                </tr>
            </thead>
            <tbody ng-repeat="course in courseList">
                <tr>
                    <td>{{course.Id}}</td>
                    <td>{{course.courseName}}</td>
                    <td>{{course.courseCode}}</td>

                    <td><button class="btn btn-warning" data-toggle="modal" data-target="#exampleModal" ng-click="editCourse(course.Id)">Edit</button></td>
                    <td><button class="btn btn-danger" ng-click="deleteCourse(course)">Delete </button></td>

                </tr>
            </tbody>
        </table>


       
        <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">UPDATE DATA</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <div class="form-group">
                            <label for="Name">Course Name:</label>
                            <input type="text" class="form-control" placeholder="Course Name" ng-model="course_name">
                        </div>
                        <div class="form-group">
                            <label for="Code">Course Code:</label>
                            <input type="text" class="form-control" placeholder="Course Code" ng-model="course_code">
                        </div>

                        <div class="modal-footer">
                            <button type="button" class="btn btn-warning" data-dismiss="modal">Close</button>
                            <button type="button" class="btn btn-danger" ng-click="saveChanges()">Save changes</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
