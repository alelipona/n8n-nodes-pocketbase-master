Please translate this carefully into English without losing the format
![Banner image](https://user-images.githubusercontent.com/10284570/173569848-c624317f-42b1-45a6-ab09-f0ea3c247648.png)

# n8n-nodes-pocketbase-master

This package contains custom nodes for integrating PocketBase with n8n.

## Installation

### In n8n

1. Go to **Settings > Community Nodes**
2. Select **Install**
3. Enter `n8n-nodes-pocketbase-master` in the search field
4. Click install

### Manually

```bash
npm install n8n-nodes-pocketbase-master
```

## Supported Operations

### Record

The node supports the following operations for records:

#### Create
- Creates a new record in a collection
- Parameters:
  - Collection: Name of the collection
  - Data: Record data in JSON format

#### Create with Files
- Creates a new record with file uploads
- Parameters:
  - Collection: Name of the collection
  - Fields Data: Regular record fields
  - Binary Property: Binary property containing the files
  - File Field Names: Names of the fields that will receive the files

#### Delete
- Removes a specific record
- Parameters:
  - Collection: Name of the collection
  - Record ID: ID of the record to be removed

#### Get
- Retrieves a specific record
- Parameters:
  - Collection: Name of the collection
  - Record ID: ID of the record
  - Expand Relations: List of relations to be expanded

#### Get Many
- Retrieves multiple records with support for filters and pagination
- Parameters:
  - Collection: Name of the collection
  - Return All: Whether to return all results
  - Limit: Maximum number of results (if Return All is false)
  - Filter: Filter criteria
  - Sort: Sorting of results
  - Expand Relations: List of relations to be expanded

#### Multi-Table Query
- Queries records in multiple tables simultaneously
- Parameters for each table:
  - Collection Name: Name of the collection
  - Filter: Table-specific filter criteria
  - Fields: Fields to be returned
  - Expand Relations: List of relations to be expanded
  - Limit: Maximum number of results
- Additional options:
  - Merge Results: Combines results based on a common field
  - Merge Field: Field used to combine results
  - Remove Duplicates: Removes duplicate entries when combining

#### Update
- Updates an existing record
- Parameters:
  - Collection: Name of the collection
  - Record ID: ID of the record
  - Data: New data in JSON format

#### Update with Files
- Updates an existing record with file support
- Parameters:
  - Collection: Name of the collection
  - Record ID: ID of the record
  - Fields Data: Regular record fields
  - Binary Property: Binary property containing the files
  - File Field Names: Names of the fields that will receive the files
  - Append Files: Whether to append or replace existing files

## Relation Expansion

The node now supports relation expansion in several operations (Get, Get Many, Multi-Table Query). This allows fetching related data in a single query.

### Usage Examples

1. **Fetch student with their class and school:**
   ```
   Expand Relations: "class,class.school"
   ```

2. **Fetch class with all students and teacher:**
   ```
   Expand Relations: "students,teacher"
   ```

3. **Fetch school with classes and their respective teachers:**
   ```
   Expand Relations: "classes,classes.teacher"
   ```

### Expansion Syntax

- Use comma (,) to expand multiple relations
- Use dot (.) to expand nested relations
- Example: "relation1,relation2.subrelation,relation3"

## Authentication

The node supports two authentication methods:

1. **Email/Password**
   - Use PocketBase admin credentials
   - Recommended for development and testing

2. **API Token**
   - Use an API token generated in PocketBase
   - Recommended for production

## Error Handling

The node includes robust error handling and validations:
- Authentication verification
- Parameter validation
- Descriptive error messages
- Option to continue on failure

## Examples

### Fetch Students from a Specific Class

```json
{
  "operation": "getMany",
  "collection": "students",
  "filter": "class.id = 'abc123'",
  "expand": "class,class.school"
}
```

### Update Student Profile Photo

```json
{
  "operation": "updateWithFiles",
  "collection": "students",
  "recordId": "xyz789",
  "fileFieldNames": "profile_photo",
  "appendFiles": false
}
```

### Multi-Table Query with Relations

```json
{
  "operation": "multiTableQuery",
  "tables": [
    {
      "collection": "students",
      "expand": "class",
      "filter": "active = true"
    },
    {
      "collection": "teachers",
      "expand": "classes,department",
      "filter": "status = 'active'"
    }
  ],
  "options": {
    "mergeResults": true,
    "mergeField": "class_id"
  }
}
```

## License

[MIT](LICENSE.md)
