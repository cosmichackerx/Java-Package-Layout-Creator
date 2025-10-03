# Java-Package-Layout-Creator
```java
import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Scanner;

public class PackageLayoutCreator {
    
    private static final String DEFAULT_SOURCE_DIR = "src";
    
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("=== Java Package Layout Creator ===\n");
        
        // Get base directory
        System.out.print("Enter base directory (press Enter for 'src'): ");
        String baseDir = scanner.nextLine().trim();
        if (baseDir.isEmpty()) {
            baseDir = DEFAULT_SOURCE_DIR;
        }
        
        System.out.println("\nPaste your package paths (one per line).");
        System.out.println("Example: com/ssms/app");
        System.out.println("Type 'done' when finished:\n");
        
        int createdCount = 0;
        int skippedCount = 0;
        
        while (scanner.hasNextLine()) {
            String line = scanner.nextLine().trim();
            
            if (line.equalsIgnoreCase("done")) {
                break;
            }
            
            if (line.isEmpty()) {
                continue;
            }
            
            try {
                if (createPackageDirectory(baseDir, line)) {
                    System.out.println("✓ Created: " + baseDir + File.separator + line);
                    createdCount++;
                } else {
                    System.out.println("○ Already exists: " + baseDir + File.separator + line);
                    skippedCount++;
                }
            } catch (IOException e) {
                System.err.println("✗ Error creating " + line + ": " + e.getMessage());
            }
        }
        
        System.out.println("\n=== Summary ===");
        System.out.println("Created: " + createdCount + " directories");
        System.out.println("Skipped (already exist): " + skippedCount + " directories");
        
        scanner.close();
    }
    
    /**
     * Creates a package directory structure
     * @param baseDir Base directory (e.g., "src")
     * @param packagePath Package path (e.g., "com/ssms/app")
     * @return true if directory was created, false if it already existed
     * @throws IOException if directory creation fails
     */
    private static boolean createPackageDirectory(String baseDir, String packagePath) throws IOException {
        // Normalize path separators
        packagePath = packagePath.replace('\\', '/').replace('.', '/');
        
        Path fullPath = Paths.get(baseDir, packagePath);
        
        if (Files.exists(fullPath)) {
            return false;
        }
        
        Files.createDirectories(fullPath);
        return true;
    }
    
    /**
     * Batch create multiple packages from an array
     * Useful for programmatic creation
     */
    public static void createBatchPackages(String baseDir, String[] packages) {
        System.out.println("Creating package layout under: " + baseDir + "\n");
        
        int created = 0;
        int skipped = 0;
        
        for (String pkg : packages) {
            try {
                if (createPackageDirectory(baseDir, pkg)) {
                    System.out.println("✓ Created: " + baseDir + File.separator + pkg);
                    created++;
                } else {
                    System.out.println("○ Already exists: " + baseDir + File.separator + pkg);
                    skipped++;
                }
            } catch (IOException e) {
                System.err.println("✗ Error creating " + pkg + ": " + e.getMessage());
            }
        }
        
        System.out.println("\n=== Summary ===");
        System.out.println("Created: " + created + " directories");
        System.out.println("Skipped: " + skipped + " directories");
    }
    
    // Example usage with predefined packages
    public static void createSSMSLayout() {
        String[] ssmsPackages = {
            "com/ssms/app",
            "com/ssms/entities",
            "com/ssms/enums",
            "com/ssms/interfaces",
            "com/ssms/repo",
            "com/ssms/services",
            "com/ssms/persistence",
            "com/ssms/utils",
            "com/ssms/exceptions",
            "com/ssms/annotations"
        };
        
        createBatchPackages(DEFAULT_SOURCE_DIR, ssmsPackages);
    }
}
```
## Features:

1. **Interactive Mode** - Run `main()` and paste package paths one by one
   - Type package paths like `com/ssms/app`
   - Type 'done' when finished
   - Shows ✓ for created, ○ for already existing directories

2. **Batch Mode** - Use `createBatchPackages()` method with an array of packages
   - Good for programmatic creation

3. **Preset Mode** - Call `createSSMSLayout()` for the exact SSMS structure you mentioned

## Usage:

**Compile and run:**
```bash
javac PackageLayoutCreator.java
java PackageLayoutCreator
```

**Example interaction:**
```
=== Java Package Layout Creator ===

Enter base directory (press Enter for 'src'): 
[Press Enter]

Paste your package paths (one per line).
Example: com/ssms/app
Type 'done' when finished:

com/ssms/app
com/ssms/entities
com/ssms/enums
done
```

The program handles:
- Both `/` and `\` path separators
- Converts `.` to `/` (so you can paste `com.ssms.app`)
- Skips empty lines
- Reports what was created vs. already existed
- Creates nested directories automatically
