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