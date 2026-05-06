# studentfileadd
Student Management System using PHP (CSV Storage) A simple PHP-based project to manage student records using file handling (CSV). It supports adding students, reading all students, and searching by ID without using a database
//Brain Logic File 
<?php
class Student {
    public $id;
    public $name;
    public $batch;

    public function __construct($id, $name, $batch) {
        $this->id = $id;
        $this->name = $name;
        $this->batch = $batch;
    }

    public function saveToCSV($file = 'students.csv') {
        $fp = fopen($file, 'a'); // open file first
        fputcsv($fp, [$this->id, $this->name, $this->batch]); // write data
        fclose($fp);
    }

    public static function getAll($file = 'students.csv') {
        $students = [];

        if (file_exists($file)) {
            $fp = fopen($file, 'r');

            while (($row = fgetcsv($fp)) !== false) {
                $students[] = new Student($row[0], $row[1], $row[2]);
            }

            fclose($fp);
        }

        return $students;
    }

    public static function findByID($id, $file = 'students.csv') {
        if (file_exists($file)) {
            $fp = fopen($file, 'r');

            while (($row = fgetcsv($fp)) !== false) {
                if ($row[0] == $id) {
                    fclose($fp); // close before returning
                    return new Student($row[0], $row[1], $row[2]);
                }
            }

            fclose($fp);
        }

        return null;
    }
}
?>

// Create file PHP 
<?php
require 'files/student.php';
if (isset($_POST['id']) && isset($_POST['name']) && isset($_POST['batch'])) {
    $student = new Student($_POST['id'], $_POST['name'], $_POST['batch']); // create student object
    $student->saveToCSV(); // save to csv file

    echo 'Student saved successfully.';
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Create</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <nav>
        <a href="create.php">New Student</a>
        | 
        <a href="list.php">Students List</a>
        | 
        <a href="search.php">Find Student</a>
    </nav>
    <h3>Add new student</h3>
    <form action="" method="post">
        <label for="id">ID</label><br>
        <input type="text" name="id" id="id"><br><br>
        <label for="name">Name</label><br>
        <input type="text" name="name" id="name"><br><br>
        <label for="batch">Batch</label><br>
        <input type="text" name="batch" id="batch"><br><br>
        <button type="submit">Save</button>
    </form>
</body>
</html>

//list added php 


<?php
require 'files/student.php';
$students = Student::getAll();
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>List</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <nav>
        <a href="create.php">New Student</a>
        | 
        <a href="list.php">Students List</a>
        | 
        <a href="search.php">Find Student</a>
    </nav>
    <h3>Student List</h3>
    <table border="1" width="400">
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Batch</th>
        </tr>

        <?php foreach ($students as $student) { ?>
            <tr>
                <td><?= $student->id ?></td>
                <td><?= $student->name ?></td>
                <td><?= $student->batch ?></td>
            </tr>
        <?php } ?>
    </table>
</body>
</html>

//Search php file 

<?php
require 'files/student.php';
if (isset($_GET['id'])) {
    $student = Student::findByID($_GET['id']);
    // if ($student) {
    //     echo 'ID: ' . $student->id . '<br>';
    //     echo 'Name: ' . $student->name . '<br>';
    //     echo 'Batch: ' . $student->batch . '<br>';
    // } else {
    //     echo 'Student not found.';
    // }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Search</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <nav>
        <a href="create.php">New Student</a>
        | 
        <a href="list.php">Students List</a>
        | 
        <a href="search.php">Find Student</a>
    </nav>
    <h3>Find student by ID</h3>
    <form action="" method="get">
        <input type="search" name="id" id="id">
        <button type="submit">Search</button>
    </form>

    <?php if (isset($student)) { ?>
        <h3>Student found</h3>
        ID: <?php echo $student->id; ?><br>
        Name: <?php echo $student->name; ?><br>
        Batch: <?php echo $student->batch; ?><br>
    <?php } else { ?>
        <h3>Student not found</h3>
    <?php } ?>
</body>
</html>


// Style added 


    body {
        font-family: Arial;
        background: #f4f6f9;
        margin: 0;
        padding: 0;
    }

    nav {
        background: #c72684;
        padding: 10px;
        text-align: center;
    }

    nav a {
        color: #141313;
        text-decoration: none;
        margin: 0 15px;
        font-weight: bold;
    }

    nav a:hover {
        color: #68bc1a;
    }

    .container {
        width: 200px;
        margin: 20px auto;
        background: #fff;
        padding: 15px;
        border-radius: 10px;
        box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    h3 {
        text-align: center;
        margin-bottom: 10px;
    }

    input {
        width: 100%;
        padding: 10px;
        margin-top: 5px;
        margin-bottom: 10px;
        border: 1px solid #971717;
        border-radius: 5px;
    }

    button {
        width: 100%;
        padding: 10px;
        background: #1abc9c;
        border: none;
        color: white;
        font-size: 16px;
        border-radius: 5px;
        cursor: pointer;
    }

    button:hover {
        background: #16a085;
    }

    table {
        width: 60%;
        margin: 30px auto;
        border-collapse: collapse;
        background: white;
        box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    th, td {
        padding: 5px;
        border-bottom: 1px solid #ddd;
        text-align: center;
    }

    th {
        background: #2c3e50;
        color: white;
    }

    tr:hover {
        background: #f1f1f1;
    }

    .result {
        margin-top: 20px;
        padding: 15px;
        background: #ecf9f6;
        border-left: 5px solid #1abc9c;
    }

    .error {
        margin-top: 20px;
        padding: 15px;
        background: #fdecea;
        border-left: 5px solid red;
    }
