# Java-Package-Layout-Creator

I'll create a Java program that automatically generates a project package layout by parsing and creating the directory structure.

I've created a Java program that can create package layouts in three ways:

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
