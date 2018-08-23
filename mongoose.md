#### Mongoose - Data Validation

##### Validation
##### Built-In Validators
```javascript
const mongoose = require('mongoose');
const { Schema } = mongoose;

const courseSchema = new Schema({
  name: { type: String, required: true },
  author: String,
  tags: [String],
  date: { type: Date, default: Date.now },
  isPublished: Boolean
});

const Course = mongoose.model("Course", courseSchema);

async createCourse = () => {
  const course = new Course({
    name: 'Node JS Course',
    author: 'John',
    tags: ['node', 'express'],
    isPublished: true
  });

  try {
      const result = await course.save();
    };
  } catch(err) {
    console.log(err.message)
  }
}
```



##### Custom Validators

##### Async Validators

##### Validation Errors

##### SchemaType Options

##### Add Persistence to Genres API


8.7- Project.zip
109. Project- Build the Customers API
6:59
8.8- Project- Build the Customers API.zip
110. Restructuring the Project
6:14
8.9- Project- Restructure the Project.zip
111. Recap
Mongoose Validation Recap.pdf
Section: 9
0 / 12
Mongoose- Modeling Relationships Between Connected Data
112. Modelling Relationships
7:45
113. Referencing Documents
3:51
9.2- Referencing Documents.zip
114. Population
4:16
115. Embedding Documents
6:54
9.4- Embedding Documents.zip
116. Using an Array of Sub-documents
4:31
117. Project- Build the Movies API
7:05
9.6- Project- Build the Movies API.zip
118. Project- Build the Rentals API
8:01
9.7- Project- Build the Rentals API.zip
119. Transactions
8:45
120. ObjectID
7:03
121. Validating Object ID's
6:13
9.10- Validating ObjectIDs.zip
122. A Better Implementation
2:23
123. Recap
Mongoose- Modelling Relationships between Connected Data Recap.pdf
