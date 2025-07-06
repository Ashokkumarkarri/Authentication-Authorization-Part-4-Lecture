# ✅ Commit 1: Rendering PrimeDealsSection

So in our e-commerce app we would have prime users and non-prime users. For prime users, we will have exclusive prime deals only for prime users.

For non-prime users, there will be a banner like "Get Exclusive Deals".

Whereas all the products will be common for both prime and non-prime users.

---

## GET Prime Deals API:

This API is only for prime users. Only prime users can have access to this API.

So the user needs to be authorized to access this API, which means he has to send the Bearer JWT token in the Authorization header when calling the API.

We already have the `"PrimeDealsSection"`. Now all we need to do is add this to our `Products` component so that it will be rendered in the UI. So let's import and add it to the `Products` component.

```js
import PrimeDealsSection from '../PrimeDealsSection'
import AllProductsSection from '../AllProductsSection'
import Header from '../Header'
import './index.css'
const Products = () => (
  <>
    <Header />
    <div className="product-sections">
      <PrimeDealsSection />
      <AllProductsSection />
    </div>
  </>
)
export default Products
```
📦 :In the next commit, We will focus on conditional rendering 

---
# ✅ Commit 2: API Call Status Handling On Success

The API call can also fail.  
There will be different API call views:

1. **GetPrimeDeals API Success** – Here, the user will be shown all the prime deals in the UI.
2. **API Call Failure** – We want to display a failure view. If the API call fails, that means the user may not be a prime user.  
   Failure of the **GetPrimeDeals API** can happen due to many reasons like:
   - Network issues
   - Wrong API URL
   - Or the user is not a prime user

   But for now, we assume if the API fails, the user is **not a prime user**. So, in that case, we will show something like “Get Prime Deals” (a banner).

3. **API in Progress** – When we make the API call and are waiting for the response, we want to display a **loading screen**.

---

We already saw the **success API call view**, now we will focus on the **failure view**.

We already have a **failure view component** (pre-filled code) called `renderPrimeDealsFailureView()`.

We also have a **loader** which will be shown when the API is fetching data.

Now, our **goal is to render the UI based on API status**.

So, to render the UI differently based on the API status, we need to **maintain the API status in the component state**, and then render based on that.

---

## ✅ Handling Success View

```js
class PrimeDealsSection extends Component {
  state = {
    primeDeals: [],
    apiStatus: '',
    // we are maintaining the API status
  }
```

```js
    if (response.ok === true) {
        ...
        ...
        ...

      this.setState({
        primeDeals: updatedData,
        apiStatus: 'SUCCESS', // if API is successful, update the status to SUCCESS
      })
    }


```

```js
  render() {
    const {apiStatus} = this.state
    switch (apiStatus) {
      case 'SUCCESS':
        return this.renderPrimeDealsList()
      default:
        return null
    }
  }
}

export default PrimeDealsSection


```
In the above code, the logic is working well,
but notice that we used the 'SUCCESS' string in multiple places.

So, if we want to change the string in the future, we would need to change it in multiple places.
If we forget to change in one place, we might get an error or bug.

✅ Best Practice:
Instead of using the same string in multiple places, the industry standard is to store such constants in one place and reuse the variable.

In the next commit, we will replace the strings and show you how to store them in one place and use them throughout the app.

---
# ✅ Commit 3: Using Constants for API Status

As we planned in the previous commit, we created the `apiStatusConstants` object and used it in the code.  
This way, we replaced the hard-coded strings used in multiple places.

---

```js
const apiStatusConstants = {
  success: 'SUCCESS',
}
```
Instead of using 'SUCCESS' string directly, we now use:

```js
this.setState({
  primeDeals: updatedData,
  apiStatus: apiStatusConstants.success, // if the API call is success then we are updating the state to success, so that we can render the UI accordingly.
})
```
Then, while rendering based on API status:
```js
render() {
  const {apiStatus} = this.state
  switch (apiStatus) {
    case apiStatusConstants.success:
      return this.renderPrimeDealsList()
    default:
      return null
  }
}

```

Using constants like this helps us in the future.
If we want to change 'SUCCESS' to something else, we just change it in one place (inside apiStatusConstants) instead of searching and changing it in many places.

✅ In the upcoming commit, we will handle failure view and loading view.

---
# ✅ Commit 4: Handling Failure View

So we are done with handling the success view.  
Now we will handle the **failure view**:

---

### ❓ What are the reasons for API call failure?

There could be multiple reasons. A few are:

1. Sending unauthorized user credentials  
   (Ex: a non-prime user trying to access the Prime Deals API)

2. Not specifying the Authorization headers

3. Using the wrong HTTP method (Ex: POST instead of GET)

...and more.

But in our project, we are focusing only on one reason:  
➡️ **Sending unauthorized user credentials**

---

### ✅ Updated Constants

```js
const apiStatusConstants = {
  success: 'SUCCESS',
  failure: 'FAILURE',
}
```
✅ Update API Status when failure
```
} else if (response.status === 401) {
  // if API call failed, we will get status code 401 (Unauthorized)

  this.setState({apiStatus: apiStatusConstants.failure})
}
```
✅ Render failure view based on API status

```js
case apiStatusConstants.failure:
  return this.renderPrimeDealsFailureView()

```

✅ So we are now done with handling failure view.

🔜 In the next commit, we will handle the loading view —
i.e., when the API call is in progress, we want to show a loading spinner.


```