```
<?php
session_start();
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL);

// Ensure the vendor directory is correctly referenced
require __DIR__ . '/vendor/autoload.php';
use PhpOffice\PhpSpreadsheet\IOFactory;

// Define data directory
$DATA_DIR = __DIR__ . '/data/';

// Ensure the data directory exists
if (!is_dir($DATA_DIR)) {
    if (!@mkdir($DATA_DIR, 0755, true)) {
        die("<div class='alert alert-danger'>Failed to create data directory. Error: " . error_get_last()['message'] . "</div>");
    }
}

// Check if the data directory is writable
if (!is_writable($DATA_DIR)) {
    $perms = substr(sprintf('%o', fileperms($DATA_DIR)), -4);
    $owner = posix_getpwuid(fileowner($DATA_DIR))['name'];
    $group = posix_getgrgid(filegroup($DATA_DIR))['name'];
    $currentUser = get_current_user();
    die("<div class='alert alert-danger'>
        The 'data' directory is not writable. <br>
        Current permissions: $perms <br>
        Owner: $owner <br>
        Group: $group <br>
        Current PHP user: $currentUser <br>
        Please ensure the directory is writable by the web server.
    </div>");
}

$excelFile = null;
$sheets = [];

// Handle file upload
if (isset($_FILES['excel_file']) && $_FILES['excel_file']['error'] === UPLOAD_ERR_OK) {
    $file = $_FILES['excel_file'];
    $allowedTypes = ['application/vnd.openxmlformats-officedocument.spreadsheetml.sheet', 'application/vnd.ms-excel'];
    
    if (in_array($file['type'], $allowedTypes)) {
        $fileName = uniqid() . '.xlsx';
        $filePath = $DATA_DIR . $fileName;
        
        if (move_uploaded_file($file['tmp_name'], $filePath)) {
              echo "<div class='alert alert-success'>File uploaded successfully.</div>";
               $_SESSION['excel_file'] = $filePath;
              $excelFile = $filePath;
           
        } else {
            $uploadError = error_get_last();
            $perms = substr(sprintf('%o', fileperms($DATA_DIR)), -4);
            echo "<div class='alert alert-danger'>
                Failed to move the uploaded file. <br>
                Error: " . ($uploadError ? $uploadError['message'] : "Unknown error") . " <br>
                Destination directory permissions: $perms <br>
                PHP User: " . get_current_user() . " <br>
                Please check the server logs for more information.
            </div>";
        }
    } else {
        echo "<div class='alert alert-danger'>Invalid file type. Please upload an Excel file.</div>";
    }
} else {
   if (isset($_SESSION['excel_file']) && file_exists($_SESSION['excel_file'])) {
        $excelFile = $_SESSION['excel_file'];
    } else {
        // Use the most recent file from the data directory
        $files = glob($DATA_DIR . '*.xlsx');
         if (!empty($files)) {
             $_SESSION['excel_file'] = end($files);
            $excelFile = $_SESSION['excel_file'];
        }
    }
}

// Load sheet list if requested
if (isset($_POST['action']) && $_POST['action'] === 'get_sheets') {
      if ($excelFile !== null) {
        try {
            if (!file_exists($excelFile)) {
               echo '<li class="nav-item"><span>No file uploaded.</span></li>';
            } else {
                 $spreadsheet = IOFactory::load($excelFile);
                $sheets = $spreadsheet->getSheetNames();
                 if (!empty($sheets)) {
                   foreach ($sheets as $index => $sheet) {
                       echo '<li class="nav-item">';
                       echo '<a class="nav-link ' . ($index === 0 ? 'active' : '') . '" href="#" data-sheet="' . htmlspecialchars($sheet) . '">';
                        echo '<i class="fas fa-table mr-2"></i>' . htmlspecialchars($sheet) . '</a>';
                        echo '</li>';
                    }
                }else{
                   echo '<li class="nav-item"><span>No sheets available.</span></li>';
                  }
            }
        } catch (Exception $e) {
            echo '<li class="nav-item"><span>Error loading sheets: ' . htmlspecialchars($e->getMessage()) . '</span></li>';
        }
    } else {
        echo '<li class="nav-item"><span>No file uploaded.</span></li>';
    }
    exit;
}

// Load sheet data if requested
if (isset($_POST['action']) && $_POST['action'] === 'load_sheet' && $excelFile !== null) {
    header('Content-Type: text/html');
    $sheetName = filter_input(INPUT_POST, 'sheet', FILTER_SANITIZE_FULL_SPECIAL_CHARS);
    try {
        if (!file_exists($excelFile)) {
            die('<div class="alert alert-danger">File not found: ' . htmlspecialchars($excelFile) . '</div>');
        }
        $spreadsheet = IOFactory::load($excelFile);
        $sheet = $spreadsheet->getSheetByName($sheetName);
        if (!$sheet) {
            $availableSheets = $spreadsheet->getSheetNames();
            die('<div class="alert alert-danger">Sheet not found. Available sheets: ' . implode(', ', $availableSheets) . '</div>');
        }
        $highestRow = $sheet->getHighestRow();
        $highestColumn = $sheet->getHighestColumn();
         if ($highestRow == 1 && $highestColumn == "A") {
            die('<div class="alert alert-info">Sheet is empty.</div>');
        }

        echo '<table class="table table-striped table-hover">';
        echo '<thead><tr>';
        for ($col = 'A'; $col <= $highestColumn; $col++) {
            $cellValue = $sheet->getCell($col . '1')->getValue();
            echo '<th>' . htmlspecialchars($cellValue ?? '') . '</th>';
        }
        echo '</tr></thead>';
        echo '<tbody>';
        for ($row = 2; $row <= $highestRow; $row++) {
            echo '<tr>';
            for ($col = 'A'; $col <= $highestColumn; $col++) {
                $cellValue = $sheet->getCell($col . $row)->getValue();
                echo '<td>' . htmlspecialchars($cellValue ?? '') . '</td>';
            }
            echo '</tr>';
        }
        echo '</tbody>';
        echo '</table>';
    } catch (Exception $e) {
        die('<div class="alert alert-danger">Error: ' . htmlspecialchars($e->getMessage()) . '</div>');
    }
    exit;
}

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Excel Data Dashboard</title>
    <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css" rel="stylesheet">
    <style>
        .sidebar {
            position: fixed;
            top: 0;
            bottom: 0;
            left: 0;
            z-index: 100;
            padding: 48px 0 0;
            box-shadow: inset -1px 0 0 rgba(0,0,0,.1);
        }
        .sidebar-sticky {
            position: relative;
            top: 0;
            height: calc(100vh - 48px);
            padding-top: .5rem;
            overflow-x: hidden;
            overflow-y: auto;
        }
        .main-content {
            margin-left: 250px;
            padding: 20px;
        }
        .page-header {
            margin-top: 0;
        }
        @media (max-width: 767.98px) {
            .sidebar {
                position: static;
                padding-top: 20px;
            }
            .main-content {
                margin-left: 0;
            }
        }
         .nav-link.active {
            background-color: #f0f0f0;
            font-weight: bold;
        }
    </style>
</head>
<body>
<div class="container-fluid">
    <div class="row">
        <!-- Sidebar -->
        <nav class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse">
            <div class="sidebar-sticky pt-3">
                <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
                    <span>Sheets</span>
                </h6>
                <ul class="nav flex-column" id="sheetList">
                    <!-- Sheet list will be loaded here -->
                </ul>
            </div>
        </nav>
        <!-- Main content -->
        <main role="main" class="col-md-9 ml-sm-auto col-lg-10 px-md-4 main-content">
            <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
                <h1 class="h2">Excel Data Dashboard</h1>
            </div>
            <form action="" method="post" enctype="multipart/form-data" class="mb-3">
                <div class="custom-file">
                    <input type="file" class="custom-file-input" id="excelFile" name="excel_file" accept=".xlsx">
                    <label class="custom-file-label" for="excelFile">Choose file...</label>
                </div>
                <button type="submit" class="btn btn-primary mt-2">Upload</button>
            </form>
            <div class="input-group mb-3">
                <div class="input-group-prepend">
                    <span class="input-group-text"><i class="fas fa-search"></i></span>
                </div>
                <input type="text" id="searchInput" class="form-control" placeholder="Search...">
            </div>
            <div id="content">
                 <!-- Content will be shown here -->
            </div>
        </main>
    </div>
</div>
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.3/dist/umd/popper.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
<script>
    $(document).ready(function() {
        // Function to load sheet content
        function loadSheet(sheetName) {
            $.ajax({
                url: 'index.php',
                type: 'POST',
                data: { action: 'load_sheet', sheet: sheetName },
                dataType: 'html',
                beforeSend: function() {
                    $('#content').html('<div class="text-center"><i class="fas fa-spinner fa-spin fa-3x"></i></div>');
                },
                success: function(data) {
                    $('#content').html(data);
                },
                error: function(jqXHR, textStatus, errorThrown) {
                    $('#content').html('<div class="alert alert-danger">Error loading data: ' + textStatus + ' - ' + errorThrown + '</div>');
                }
            });
        }

        // Function to load sheet list
        function loadSheets() {
            $.ajax({
                url: 'index.php',
                type: 'POST',
                data: { action: 'get_sheets' },
                dataType: 'html',
                success: function(data) {
                    $('#sheetList').html(data);
                     var firstSheet = $('a.nav-link').first();
                      if (firstSheet.length) {
                        loadSheet(firstSheet.data('sheet'));
                        firstSheet.addClass('active');
                     } else {
                           $('#content').html('<div class="alert alert-info">No sheets available.</div>');
                        }
                },
                error: function(jqXHR, textStatus, errorThrown) {
                    $('#sheetList').html('<li class="nav-item"><span>Error loading sheets.</span></li>');
                }
            });
        }

        // Handle sheet link click
        $(document).on('click', 'a.nav-link', function(e) {
            e.preventDefault();
             var sheetName = $(this).data('sheet');
             loadSheet(sheetName);
             $('a.nav-link').removeClass('active');
             $(this).addClass('active');
        });

         // Handle file upload
        $('form').on('submit', function(e) {
            if ($(this).find('.custom-file-input')[0].files.length > 0) {
                e.preventDefault();
                $(this).unbind('submit').submit();
                 $('#content').empty();
                 loadSheets();
            }
        });
         // Handle file input label change
        $('.custom-file-input').on('change', function() {
            let fileName = $(this).val().split('\\').pop();
            $(this).next('.custom-file-label').addClass("selected").html(fileName);
        });
        // Handle search input
        $('#searchInput').on('keyup', function() {
            var searchText = $(this).val().toLowerCase();
            $('#content tr').each(function() {
                var rowText = $(this).text().toLowerCase();
                if (rowText.indexOf(searchText) === -1) {
                    $(this).hide();
                } else {
                    $(this).show();
                }
            });
        });

        // Initial load
        loadSheets();
    });
</script>
</body>
</html>
```
