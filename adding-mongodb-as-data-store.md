## Add Mongo as data store
1. Create app/cluster in [MongoDb cloud](www.mongodb.com)
2. Install extra packages
```shell
npm install mongoose
```
3. Add Mongo URI from MongoDB Cloud to `.env`
```
MONGO_URI={URI}
```
4. Create the db configuration inside `src/config/db.ts`
```ts
import mongoose from "mongoose";

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGO_URI ?? "");
    console.log("MongoDB Connected: ", conn.connection.host);
  } catch (err: any) {
    console.error(err.message);
    process.exit(1);
  }
};

export default connectDB;
```
5. Initiallise DB connection inside `src/app.ts`
```ts
import connectDB from "./config/db";

connectDB()
```
6. Create models inside `src/models`
```ts
import mongoose from "mongoose";

const TaskSchema = new mongoose.Schema(
  {
    user: {
      type: mongoose.Schema.Types.ObjectId,
      required: true,
      ref: "User",
    },
    name: {
      type: String,
      required: true,
    },
  },
  {
    timestamps: true,
  }
);

export default mongoose.model("Task", TaskSchema);
```
