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