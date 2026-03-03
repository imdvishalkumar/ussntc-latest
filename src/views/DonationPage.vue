<template>
    <div class="container">
      <div class="box">
        <h2 class="text-center header-font">Donation</h2>
        <div class="row align-items-center">
          <div class="col-md-5 d-flex justify-content-center">
            <img :src="selectedProductImage" style="width: 100%; height: auto;" />
          </div>
          <div class="col-md-7">
            <div class="donation-section mt-4">
              <h5 class="fw-bold">DONATION</h5>
              <hr />
              <label for="donation-amount" class="fw-bold">Select a donation amount:</label>
  
              <select v-if="donationData?.amounts" v-model="selectedAmount" class="form-control mt-2">
                <option value="" disabled>-- Please select an amount --</option>
                <option
                  v-for="(amount, key) in donationData.amounts"
                  :key="key"
                  :value="amount.price"
                >
                  {{ amount.description }}
                </option>
                <option value="other">Other Amount</option>
              </select>
  
              <!-- Show input if "Other amount" is selected -->
              <input v-if="selectedAmount === 'other'"
                v-model="customAmount"
                type="number"
                class="form-control mt-2"
                placeholder="Enter custom amount"
                min="1"
                @keypress="preventExponent"
              />
  
              <button class="btn btn-primary mt-3 w-100" @click="addToCart">Add to cart</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import { useRouter } from 'vue-router';
  import { useCartStore } from '@/store/cart';
  import { useCampaignStore } from '@/store/campaign';
  
  export default {
    setup() {
      const selectedAmount = ref('');
      const customAmount = ref('');
      const selectedProductImage = ref('https://frsengraving.com/armymuseum/images/donation600.png');
  
      const donationData = ref(null);
      const cartStore = useCartStore();
      const campaignStore = useCampaignStore();
      const router = useRouter();
  
      onMounted(() => {
        // Load donation data
        const donation = campaignStore?.campaignData?.campaign?.options?.donation;
        if (donation && donation.amounts) {
          donationData.value = donation;
          console.log('Loaded donation data:', donationData.value);
        }
      });
  
      const addToCart = () => {
            let finalAmount;
            let selectedKey = null;

            // If custom amount is selected
            if (selectedAmount.value === 'other') {
                finalAmount = parseFloat(customAmount.value);
                if (!finalAmount || finalAmount <= 0) {
                alert("Please enter a valid custom amount.");
                return;
                }
                selectedKey = "dother"; // << Set 'dother' for custom amounts
            } else {
                finalAmount = parseFloat(selectedAmount.value);
                if (!finalAmount || finalAmount <= 0) {
                alert("Please select a valid donation amount.");
                return;
                }

                // Match the amount to the correct key (like d5, d10, d50)
                for (const [key, value] of Object.entries(donationData.value.amounts)) {
                if (value.price === finalAmount) {
                    selectedKey = key;
                    break;
                }
                }
                if (!selectedKey) {
                alert("Selected amount not found.");
                return;
                }
            }

            // Build the cart item
            const cartItem = {
                gc: false,
                donation: true,
                item_id: 2337,
                children: {},
                amount_id: selectedKey,  // always matched properly now
                price: finalAmount,     // always matched properly now
                donationDescription: "Donation",
                logo_image: selectedProductImage.value, // Add image if needed
            };

            console.log('Adding to cart:', cartItem); // Check in console

            cartStore.selectedDonationAmount = finalAmount;
            cartStore.addItem(cartItem);
            router.push("/cart");
        };

      const preventExponent = (e) => {
        if (['e', 'E', '+', '-'].includes(e.key)) {
          e.preventDefault();
        }
      };
  
      return {
        selectedAmount,
        customAmount,
        donationData,
        selectedProductImage,
        addToCart,
        preventExponent,
      };
    },
  };
  </script>
  