# Commenting Conventions

## Double Slash (`//`)

- Place the comment on a separate line, not at the end of a line of code.

```csharp
// Gets the Universal Answer from the datastore (good)
int answer = DataService.ReadUniversalAnswer(); // Get universal answer (bad)
```

- Align the comment with the block or statement it is describing.
- Begin comment text with an uppercase letter.
- Insert one space between the comment delimiter (`//`) and the comment text.

```csharp
// The following declaration creates a query, but does not run it (good)
//Declares query without running it (bad)
```
- Wrap long comments to the next line as needed.

```csharp
// The following declaration creates a query. It does not run it until
// the Execute() method is called on the Query object.
```

## Triple Slash (`///`)

- Ensure all public members have the necessary XML comments beginning with three forward slashes providing appropriate descriptions about their behavior.

```csharp
/// <summary>
/// Generate a hash value to be used in determining which variation the user will be put in
/// </summary>
/// <param name="bucketingKey">string value used for the key of the murmur hash.</param>
/// <returns>integer Unsigned value denoting the hash value for the user</returns>
```
- To-do items should be marked with `/// TODO:`
- Temporary workarounds should marked with `/// KLUDGE:`

## Comment Blocks (`/* */`)

- Do not create formatted blocks of asterisks around comments unless:
  - the file needs a copyright/legal disclaimer before the source code begins
  - for multi-line testing scenarios

```csharp
/* 
 * Copyright {year} Optimizely
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

 /*
 * 1. Build the solution
 * 2. Run lint
 * 3. Run the test scenarios
 */
 ```
