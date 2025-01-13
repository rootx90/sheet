```
<?php
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
    mkdir($DATA_DIR, 0755, true);
}

$excelFile = null;

// Handle file upload
if (isset($_FILES['excel_file']) && $_FILES['excel_file']['error'] === UPLOAD_ERR_OK) {
    $file = $_FILES['excel_file'];
    $allowedTypes = ['application/vnd.openxmlformats-officedocument.spreadsheetml.sheet', 'application/vnd.ms-excel'];
    
    if (in_array($file['type'], $allowedTypes)) {
        $fileName = uniqid() . '.xlsx';
        $filePath = $DATA_DIR . $fileName;
        
        if (move_uploaded_file($file['tmp_name'], $filePath)) {
            echo "<div class='alert alert-success'>File uploaded successfully.</div>";
            $excelFile = $filePath;
        } else {
            echo "<div class='alert alert-danger'>Failed to move the uploaded file.</div>";
        }
    } else {
        echo "<div class='alert alert-danger'>Invalid file type. Please upload an Excel file.</div>";
    }
} else {
    // Use the most recent XLSX file from the directory
    $files = glob($DATA_DIR . '*.xlsx');
    if (!empty($files)) {
        $excelFile = end($files);
    }
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
            die('<div class="alert alert-danger">Sheet not found.</div>');
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

// Generate sheet list
$sheets = [];
if ($excelFile !== null) {
    try {
        if (!file_exists($excelFile)) {
            echo "<div class='alert alert-danger'>File not found: " . htmlspecialchars($excelFile) . "</div>";
        } else {
            $spreadsheet = IOFactory::load($excelFile);
            $sheets = $spreadsheet->getSheetNames();
        }
    } catch (Exception $e) {
        echo "<div class='alert alert-danger'>Error loading Excel file: " . htmlspecialchars($e->getMessage()) . "</div>";
    }
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
                <ul class="nav flex-column">
                    <?php foreach ($sheets as $sheet): ?>
                        <li class="nav-item">
                            <a class="nav-link" href="#" data-sheet="<?php echo htmlspecialchars($sheet); ?>">
                                <i class="fas fa-table mr-2"></i><?php echo htmlspecialchars($sheet); ?>
                            </a>
                        </li>
                    <?php endforeach; ?>
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
            <div id="content"></div>
        </main>
    </div>
</div>
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.3/dist/umd/popper.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
<script>
    $(document).ready(function() {
        // Handle sheet link click
        $('a.nav-link').click(function(e) {
            e.preventDefault();
            var sheetName = $(this).data('sheet');
            loadSheet(sheetName);
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

        // Load the first sheet by default if available
        var firstSheet = $('a.nav-link').first().data('sheet');
        if (firstSheet) {
            loadSheet(firstSheet);
        }
    });
</script>
</body>
</html>



```
