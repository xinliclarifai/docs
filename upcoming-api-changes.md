## Upcoming API Changes for Clarifai

Here is a list of changes to the API that we want you to be aware of well in advance as they may
affect how you use Clarifai's platform. These changes include scheduled downtime and other
improvements in stability, performance or functionality of the Clarifai
platform in order to better serve you as a customer. Some of these changes may not be backward
compatible and thus require you to update how you call our APIs. We created this page with the
mindset of being as transparent as possible so you can plan any corresponding changes in advance and
minimize any interruptions to your usage of Clarifai.

The dates listed in the following tables are the date we plan to make the change. We may actually
make the change in the days following the specified date. However, to be safe, your client-side code
needs updating before that date to minimize any downtime to your applications.

We will continue to update this page regularly, so a good way to always stay up to date is to
watch our [documentation repo on GitHub](https://github.com/Clarifai/docs).

### Completed Changes

Coming soon...

### Upcoming Changes

| Date | Change |
| ------ | ---- |
| September 11, 2019. 9:00am ET | **Scheduled Database Downtime**<br><br>We plan to upgrade our database to make it faster and provide more space for your applications. We expect a few minutes of downtime during this upgrade but you should plan for up to an hour of downtime in case things don't go as expected. This will primarily affect the following uses of our platform:<ul><li>POST/GET/PATCH/DELETE inputs</li><li>Search</li><li>Custom Training</li><li>Model Evaluation</li></ul> |
| September 12, 2019 | **`POST /inputs` will only operate asynchronously**<br><br>We are cleaning up some inconsistent behavior in the API where a single image added with `POST /inputs` was a synchronous operation, but a batch of images was asynchronous. We are making both asynchronous. This allows us to provide more advanced functionality with workflows that index your inputs.<br><br>What this means for your code is if you application relies on added inputs having already been indexed when the `POST /inputs` call returns, you now need to add a second call to `GET /inputs/{input_id}` in order to check the status of the input you just added to look for SUCCESS status code.  |
| September 12, 2019 | **`DELETE /inputs` will only operate asynchronously**<br><br>Along the same lines as `POST /inputs` becoming completely asynchronous, we are cleaning up some inconsistent behavior in the API for deleting inputs. Previously, when a single image is deleted with `DELETE /inputs` or `DELETE /inputs/{input_id}` it was a synchronous operation, but when a batch of images were deleted it was asynchronous. We are making both asynchronous. This allows us to provide more advanced functionality with workflows that index your inputs.<br><br>What this means for your code is if you application relies on the input having been deleted when the `DELETE /inputs` or `DELETE /inputs/{input_id}` calls return, you now need to add a second call to `GET /inputs/{input_id}` in order to check that it fails with a not found error.  |
| September 12, 2019 | **`PATCH /inputs` overwrite action change**<br><br>The overwrite action when patching inputs currently has some inconsistent behavior. If you patch `input.data.metadata` or `input.data.geo` fields on an input that has `input.data.concepts` already added to it, these concepts will remain after the patch even though the patch action was `overwrite`.<br><br>Going forward, the overwrite behavior will overwrite the entire `data` object with what is included in the `PATCH /inputs` API call. Therefore if concepts are not provided in the patch call, but were originally on that input, they will be erased (overwritten with an empty list of concepts). You can maintain the current behvaiour by always sending back the complete `data` object from `GET /input/{input_id}` along with any modification to it if you are using the `overwrite` action. |
