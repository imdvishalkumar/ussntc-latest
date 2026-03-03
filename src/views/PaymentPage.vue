<template>
    <div class="container">
        <div class="box">

            <div class="row">
                <!-- Checkout Section -->
                <div class="col-lg-6 col-12 border-end border-dark no-border-end">
                    <h2 class="m-0 header-font">CHECKOUT</h2>
                    <div v-if="cartItems.length">
                        <!-- Header -->
                        <div class="row m-0 py-2 d-none d-sm-flex border-bottom border-dark">
                            <div class="col-sm-4 ps-0 fw-medium custom-title">PRODUCT</div>
                            <div class="col-sm-8">
                                <div class="row">
                                    <div class="col-7 fw-medium custom-title">DESCRIPTION</div>
                                    <div class="col-1 text-center fw-medium custom-title">QTY</div>
                                    <div class="col-4 pe-0 text-end fw-medium custom-title">SUBTOTAL</div>
                                </div>
                            </div>
                        </div>
                        <!-- Product List -->
                        <div v-for="item in cartItems" :key="item.item_id" class="py-4 border-bottom border-dark">                            
                            <div class="row">
                                <div class="col-sm-4">
                                    <div class="border-bottom border-dark mb-3 py-2 d-block d-sm-none fw-medium">PRODUCT
                                    </div>
                                    <div class="prod-container overlay-section">
                                        <img :src="item.logo_image" alt="Product Image"
                                            class="img-fluid w-100 overlay-image">
                                        <div v-if="isLogoAllowed && item?.productDescription?.toLowerCase().includes('logo')">
                                            <div class="overlay-text" :style="{ top: '70%' }">
                                                <p class="m-0" v-for="n in item.max_lines" :key="n" :class="{ 'is-max': item?.max_characters > 16 }">
                                                    <span v-if="item.lines[n - 1]">{{ item.lines[n - 1] }}</span>
                                                    <span v-else>&nbsp;</span>
                                                </p>
                                            </div>
                                        </div>
                                        <div v-else>
                                            <div class="overlay-text" :style="{ top: '50%' }">
                                                <p class="m-0" v-for="n in item.max_lines" :key="n" :class="{ 'is-max': item?.max_characters > 16 }">
                                                    <span v-if="item.lines[n - 1]">{{ item.lines[n - 1] }}</span>
                                                    <span v-else>&nbsp;</span>
                                                </p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-sm-8">
                                    <div class="d-flex border-bottom border-dark mb-3 pt-3 pb-2 d-block d-sm-none">
                                        <div class="col-sm-6 col-6 custom-title fw-medium">DESCRIPTION</div>
                                        <div class="col-sm-3 col-3 text-center custom-title fw-medium">QTY</div>
                                        <div class="col-sm-3 col-3 text-end custom-title fw-medium">SUBTOTAL</div>
                                    </div>
                                    <!-- <div class="row">
                                        <div class="col-7">{{ item.item_id === 100145 ? "DONATION" : item.productDescription }}</div>
                                        <div class="col-1 text-center">{{ item.qty }}</div>
                                        <div class="col-4 text-end">{{ item.item_id === 100145 ? formatPrice(item.donationPrice) : formatPrice(item.productPrice) }}</div>
                                    </div> -->
                                    <div class="row">
                                        <div class="col-7">{{ item.donationDescription === "Donation" ? item.donationDescription : item.productDescription }}</div>
                                        <div class="col-1 text-center">{{ item.qty }}</div>
                                        <div class="col-4 text-end">{{ item.donationDescription === "Donation" ? formatPrice(item.price) : formatPrice(item.productPrice) }}</div>
                                    </div>
                                    <div class="row"
                                        v-for="child in item.childItems.filter(child => child.quantity > 0)"
                                        :key="child.itemId">
                                        <!-- <div class="row" v-for="child in item.childItems" :key="child.itemId"> -->
                                        <div class="col-7">{{ child.itemDescription }}</div>
                                        <div class="col-1 text-center">{{ child.quantity }}</div>
                                        <div class="col-4 text-end">{{ formatPrice(child.quantity * child.itemPrice)
                                            }}
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- Price Breakdown Section -->
                    <div class="mt-4 p-3 bg-light-subtle rounded border border-dark border-opacity-10">
                        <table class="table table-borderless m-0">
                            <tbody>
                                <tr class="border-bottom border-dark border-opacity-10">
                                    <th class="text-start fw-normal py-2" scope="row">Subtotal</th>
                                    <td class="text-end py-2">${{ formatPrice(subTotal) }}</td>
                                </tr>
                                <tr v-if="discountAmount > 0" class="border-bottom border-dark border-opacity-10">
                                    <th class="text-start fw-normal py-2" scope="row">Promotional Discount</th>
                                    <td class="text-end py-2 text-danger">-${{ formatPrice(discountAmount) }}</td>
                                </tr>
                                <tr class="border-bottom border-dark border-opacity-10">
                                    <th class="text-start fw-normal py-2" scope="row">Convenience Fee</th>
                                    <td class="text-end py-2">${{ formatPrice(convenienceAmount) }}</td>
                                </tr>
                                <tr class="border-bottom border-dark border-opacity-10">
                                    <th class="text-start fw-normal py-2" scope="row">Sales Tax</th>
                                    <td class="text-end py-2">${{ formatPrice(salesTaxAmount) }}</td>
                                </tr>
                                <tr class="fs-5">
                                    <th class="text-start fw-bold pt-3" scope="row">Order Total</th>
                                    <td class="text-end fw-bold pt-3 text-primary">${{ formatPrice(finalTotal) }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>

                <!-- Payment Information Section -->
                <div class="col-lg-6 col-12">
                    <h2 class="sub-title m-0 header-font">Payment Information</h2>

                    <div class="embed-responsive embed-responsive-4by3 quantum-form" v-if="quantum !== null">
                        <iframe :key="iframeKey" :src="iframeUrl" className="quantum-payment-iframe" :allow-cookies="true" width="100%" height="500"></iframe>
                        
                        <div class="payment-logos text-center">
                            <img src="../assets/images/visa.png" alt="Visa" class="payment-icon">
                            <img src="../assets/images/mastercard.png" alt="MasterCard" class="payment-icon">
                            <img src="../assets/images/amex.png" alt="American Express" class="payment-icon">
                        </div>
                    </div>

                    <form @submit.prevent="handlePayment" class="info-form" v-else-if="authorize !== null">
                        <div class="row mb-3">
                            <div class="col-12">
                                <label class="pb-2">Credit Card</label>
                                <input v-model="paymentData.creditcard" type="text" name="creditcard"
                                    placeholder="Credit Card Number" maxlength="16"
                                    inputmode="numeric" autocomplete="cc-number"
                                    :class="['form-control', 'rounded-0', 'bg-transparent', errorPaymentClass('creditcard')]"
                                    @input="clearPaymentError('creditcard')">
                                <span v-if="paymentErrors.creditcard" class="text-danger mt-1 d-block">{{ paymentErrors.creditcard }}</span>
                            </div>
                        </div>
                        <div class="row mb-3">
                            <div class="col-sm-6 mb-3 mb-sm-0">
                                <label for="expiration" class="pb-2">Expiration (YYYY/MM)</label>
                                <input v-model="paymentData.expiration" type="text" id="expiration" name="expiration"
                                    placeholder="YYYY/MM" maxlength="7"
                                    inputmode="numeric" autocomplete="cc-exp"
                                    :class="['form-control', 'rounded-0', 'bg-transparent', errorPaymentClass('expiration')]"
                                    @input="formatExpirationDate" />
                                <span v-if="paymentErrors.expiration" class="text-danger mt-1 d-block">{{ paymentErrors.expiration }}</span>
                            </div>
                            <div class="col-sm-6">
                                <label class="pb-2">CVV Code</label>
                                <input v-model="paymentData.cvvcode" type="text" name="cvvcode" placeholder="CVV"
                                    maxlength="4" inputmode="numeric" autocomplete="cc-csc"
                                    :class="['form-control', 'rounded-0', 'bg-transparent', errorPaymentClass('cvvcode')]"
                                    @input="clearPaymentError('cvvcode')">
                                <span v-if="paymentErrors.cvvcode" class="text-danger mt-1 d-block">{{ paymentErrors.cvvcode }}</span>
                            </div>
                        </div>

                        <!-- CAPTCHA -->
                        <div class="row mb-3">
                            <div class="col-12">
                                <label class="pb-2">Captcha</label>
                                <div class="d-flex align-items-center mb-2">
                                    <span class="captcha-text px-4 py-2 fw-bold no-select rounded text-center" style="letter-spacing: 2px;">{{ captchaText }}</span>
                                    <button type="button" @click="generateCaptcha" class="btn ms-3 py-2 px-3">Refresh</button>
                                </div>
                                <input v-model="paymentData.captcha" type="text" name="captcha" placeholder="Enter Captcha"
                                    maxlength="6"
                                    :class="['form-control', 'rounded-0', 'bg-transparent', errorPaymentClass('captcha')]">
                                <span v-if="paymentErrors.captcha" class="text-danger mt-1 d-block">{{ paymentErrors.captcha }}</span>
                            </div>
                        </div>

                        <div class="text-center py-4">
                            <button @click="handlePayment" :disabled="isPlacingOrder" class="btn px-5 py-2 fw-bold d-inline-flex align-items-center justify-content-center">
                                <span v-if="isPlacingOrder" class="spinner-border spinner-border-sm me-2" role="status" aria-hidden="true"></span>
                                {{ isPlacingOrder ? 'Processing...' : 'Pay Now' }}
                            </button>
                            <p v-if="statusMessage" class="mt-2 mb-0 fw-medium" :class="{ 'text-success': !isError, 'text-danger': isError }">{{ statusMessage }}</p>
                        </div>

                        <div class="payment-logos text-center">
                            <img src="../assets/images/visa.png" alt="Visa" class="payment-icon">
                            <img src="../assets/images/mastercard.png" alt="MasterCard" class="payment-icon">
                            <img src="../assets/images/amex.png" alt="American Express" class="payment-icon">
                        </div>
                    </form>
                </div>
            </div>
            <div class="modal fade" id="removeItemModal" data-bs-backdrop="true" tabindex="-1" aria-labelledby="removeItemModalLabel" aria-hidden="true">
                <div class="modal-dialog modal-dialog-centered">
                    <div class="modal-content">
                        <div class="modal-header border-0 pb-0">
                            <button type="button" 
                            class="btn-close shadow-none border-0 no-hover"
                            data-bs-dismiss="modal" 
                            aria-label="Close">
                            </button>
                        </div>
                        <div class="modal-body pt-0 pb-4">
                            <h2 class="text-center header-font text-danger mb-3">Declined Payment</h2>
                            <p class="text-center mb-0 fs-5">{{ declineReasonMessage }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { computed, ref, watch, reactive, onMounted } from 'vue';
import { useCartStore } from '@/store/cart';
import { useCampaignStore } from '@/store/campaign';
import { useAddressStore } from '@/store/address';
import { useRouter } from 'vue-router';
import { setButtonStyles, setHeaderFontStyles, setSubHeaderFontStyles } from '@/utils/buttonStyles';
import { usePaymentStore } from '@/store/payment';
// import { FriendlyIframe } from '@/views/iframe/friendly-iframe.vue';

// processPayment
export default {
    name: 'PaymentPage',
    setup() {
        const router = useRouter();
        const cartStore = useCartStore();
        const campaignStore = useCampaignStore();
        const addressStore = useAddressStore();
        const paymentStore = usePaymentStore();

        // Add a ref to control iframe refresh
        const iframeKey = ref(0);
        const refreshIframe = () => {
        // Increment the key to force iframe refresh
        iframeKey.value += 1;
        };

        const campaign = computed(() => campaignStore.campaign);
        const campaig_short_url_name = campaign.value.short_url;
        const authorize = campaignStore.campaign.options.payment_type.authorize_payment_environment;
        const quantum = campaignStore.campaign.options.payment_type.quantum_payment_environment;
        const isLogoAllowed = computed(() => campaignStore.multilogoAllowed);

        const cartItems = computed(() => cartStore.cartItems);
        const gateway = ref({
            params: {
                skip_shipping_info: 'Y',
                ilf_API_Style: '2',
                Require_BCOUNTRY: 'Y',
                Require_phone: 'N',
                PayInfoOnly: 'Y',
                ts: +new Date(),
            }
        });

        watch(cartItems, (newCartItems) => {
            if (newCartItems.length === 0) {
                router.push('/shop');
            }
        }, { immediate: true });

        const generalInfo = computed(() => campaignStore.generalInfo);
        setButtonStyles(generalInfo.value);
        setHeaderFontStyles(generalInfo.value);
        setSubHeaderFontStyles(generalInfo.value);

        const subTotal = computed(() => cartStore.subTotal);
        const discountAmount = cartStore.discountAmount;
        const convenienceAmount = computed(() => campaignStore.convenienceAmount);
        const salesTaxAmount = computed(() => addressStore.getSalesTaxAmount);             
        const selectedAddress = addressStore.addresses[0];

        const iframeUrl = computed(() => {
        const params = {
            k: paymentStore.orderDetails.Key,
            ip: paymentStore.orderDetails.IP,
            Amount: finalTotal.value,
            ID: paymentStore.orderDetails.orderId,
            post_return_url_approved: paymentStore.orderDetails.approvedUrl,
            post_return_url_declined: paymentStore.orderDetails.declinedUrl,
            FNAME: selectedAddress.first_name,
            LNAME: selectedAddress.last_name,
            BADDR1: selectedAddress.address1,
            BCITY: selectedAddress.city,
            BSTATE: selectedAddress.state,
            BZIP1: selectedAddress.postal_code,
            BCOUNTRY: selectedAddress.country,
            phone: selectedAddress.phone,
            BCUST_EMAIL: selectedAddress.email,
            ...gateway.value.params,
            // Add timestamp to prevent caching
            _t: Date.now()
        };

        const query = '?' + Object.keys(params)
            .map(key => `${key}=${encodeURIComponent(params[key])}`)
            .join('&');

        return paymentStore.orderDetails.paymentUrl + query;
        });

        onMounted(() => {

        window.addEventListener('message', (event) => {
            if (event.origin !== 'https://apps.frsfulfillment.com') {
            console.warn('Untrusted message origin:', event.origin);
            return;
            }

            const { event: eventType, data } = event.data;

            if (eventType === 'payment-result') {
            if (data.result === 'APPROVED') {
                router.push({ path: '/confirmation' });
            } else if (data.result === 'DECLINED') {
                declineReasonMessage.value = data.message || 'Payment Declined';
                const modalElement = document.getElementById('removeItemModal');
                const removeItemModal = new window.bootstrap.Modal(modalElement);
                removeItemModal.show();
                // Add modal hide event listener
                modalElement.addEventListener('hidden.bs.modal', () => {
                refreshIframe();
                });
            } else {
                console.log('Unhandled payment status:', data);
            }
            } else {
            console.warn('Unhandled event type:', eventType);
            }
        });
        });

        const finalTotal = computed(() => subTotal.value + convenienceAmount.value + salesTaxAmount.value - discountAmount);

        const formatPrice = (price) => `${parseFloat(price).toFixed(2)}`;

        const paymentData = ref({
            creditcard: '',
            expiration: '',
            cvvcode: '',
            captcha: '',
        });

        const paymentErrors = ref({});
        const isPlacingOrder = ref(false);
        const statusMessage = ref('');
        const isError = ref(false);
        const visible = ref(false);
        const isLoading = ref(false);
        const declineReasonMessage = ref('High Fraud Score');

        const captchaText = ref(generateRandomCaptcha());
        function generateRandomCaptcha() {
            const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
            let captcha = '';
            for (let i = 0; i < 6; i++) {
                captcha += characters.charAt(Math.floor(Math.random() * characters.length));
            }
            return captcha;
        }

        function errorPaymentClass(field) {
            return paymentErrors.value[field] ? 'border-danger shadow bg-danger-subtle' : '';
        }

        function clearPaymentError(field) {
            if (paymentErrors.value[field]) {
                delete paymentErrors.value[field];
            }
        }

        function showMessage(message, isErrorFlag) {
            statusMessage.value = message;
            isError.value = isErrorFlag;
            visible.value = true;

            setTimeout(() => {
                visible.value = false;
            }, 5000);
        }

        const billing = reactive({
            first_name: '',
            last_name: '',
            address1: '',
            city: '',
            state_prov: '',
            postal_code: '',
            country: '',
            email: ''
        });
        const orderTotal = ref(0);
        const orderId = ref('');
        const tokenKey = ref('');
        const ipAddr = ref('');

        const generateInvoiceNumber = () => {
            const timestamp = Date.now();
            const randomNum = Math.floor(Math.random() * 10000);
            return `${timestamp}-${randomNum}`;
        };

        const handlePayment = async () => {
            if (!validatePaymentForm()) {
                return;
            }

            const {
                creditcard: cardNumber,
                expiration: expiryDate,
                cvvcode: cvv,
                captcha: captchaValue
            } = paymentData.value;

            isPlacingOrder.value = true;

            try {
                const selectedAddress = addressStore.addresses[0];
                const endpoint = 'https://apps.frsfulfillment.com/api/authorize/payment';
                const paymentData = {
                    cardNumber: cardNumber,
                    expiryDate: expiryDate,
                    cvv: cvv,
                    amount: finalTotal.value,
                    orderId: paymentStore.orderDetails.orderId,
                    invoiceNumber: generateInvoiceNumber(),
                    description: 'Test123',
                    salesTaxAmount : salesTaxAmount.value,
                    firstName: selectedAddress.first_name || '',
                    lastName: selectedAddress.last_name || '',
                    company: selectedAddress.company || '',
                    address: selectedAddress.address1 || '',
                    city: selectedAddress.city || '',
                    state: selectedAddress.state || '',
                    zip: selectedAddress.postal_code || '',
                    country: selectedAddress.country || '',
                    email: selectedAddress.email || 'vishal.vaghari@imobiledesigns.com',
                    campaig_short_url_name : campaig_short_url_name || ''
                };

                const response = await fetch(endpoint, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(paymentData)
                });

                if (!response.ok) {
                    throw new Error(`Network response was not ok: ${response.status} ${response.statusText}`);
                }

                const responseText = await response.text();
                const jsonMatch = responseText.match(/\{[\s\S]*\}/);
                if (!jsonMatch) {
                    throw new Error('Invalid response format');
                }

                const data = JSON.parse(jsonMatch[0]);
                console.log('Payment Response Data:', data);
                const { success, result, status, error } = data;

                if (success && (result === 'Approved' || status === 'approved' || status === 'Approved') && status !== 'failed') {
                    // store.commit('completeTheOrder');
                    setTimeout(() => {
                        router.push({ path: '/confirmation' });
                    }, 100);
                } else {
                    // Show the decline modal
                    declineReasonMessage.value = error || result || status || 'Payment declined';
                    console.error('Payment declined reason:', declineReasonMessage.value);
                    const modalElement = document.getElementById('removeItemModal');
                    if (modalElement && window.bootstrap) {
                        const removeItemModal = new window.bootstrap.Modal(modalElement);
                        removeItemModal.show();
                    } else {
                        console.error('Modal element or Bootstrap not found');
                        alert(declineReasonMessage.value); // Fallback
                    }
                    isPlacingOrder.value = false;
                }
            } catch (error) {
                console.error('Payment processing error:', error.message);
                declineReasonMessage.value = 'An error occurred while processing your payment. Please try again.';
                const modalElement = document.getElementById('removeItemModal');
                if (modalElement && window.bootstrap) {
                    const removeItemModal = new window.bootstrap.Modal(modalElement);
                    removeItemModal.show();
                }
                isPlacingOrder.value = false;
            }
        };

        function validatePaymentForm() {

            paymentErrors.value = {};
            let isValid = true;

            if (!paymentData.value.creditcard) {
                paymentErrors.value.creditcard = 'Enter credit card number';
                isValid = false;
            } else if (!/^[0-9]{16}$/.test(paymentData.value.creditcard)) {
                paymentErrors.value.creditcard = 'Credit card number is invalid';
                isValid = false;
            }

            if (!paymentData.value.expiration) {
                paymentErrors.value.expiration = 'Enter expiration date';
                isValid = false;
            } else if (!/^\d{4}\/(0[1-9]|1[0-2])$/.test(paymentData.value.expiration)) {
                paymentErrors.value.expiration = 'Expiration date must be in YYYY/MM format';
                isValid = false;
            }

            if (!paymentData.value.cvvcode) {
                paymentErrors.value.cvvcode = 'Enter CVV code';
                isValid = false;
            } else if (!/^\d{3,4}$/.test(paymentData.value.cvvcode)) {
                paymentErrors.value.cvvcode = 'CVV code is invalid';
                isValid = false;
            }

            if (!paymentData.value.captcha) {
                paymentErrors.value.captcha = 'Please Enter Captcha';
                isValid = false;
            } else if (paymentData.value.captcha !== captchaText.value) {
                paymentErrors.value.captcha = 'Captcha is incorrect';
                isValid = false;
            }

            return isValid;
        }

        function generateCaptcha() {
            captchaText.value = generateRandomCaptcha();
        }

        function formatExpirationDate() {
            let value = paymentData.value.expiration.replace(/\D/g, ''); // Remove any non-numeric characters

            // Apply the YYYY/MM format while typing
            if (value.length > 4) {
                value = value.slice(0, 4) + '/' + value.slice(4, 6); // Format as YYYY/MM
            }

            // Update the model with the formatted value
            paymentData.value.expiration = value;

            // Check if the value has a length of 5 (YYYY/MM format)
            if (value.length === 7) {
                const expirationRegex = /^\d{4}\/(0[1-9]|1[0-2])$/;

                if (!expirationRegex.test(value)) {
                    paymentErrors.value.expiration = 'Invalid expiration date format';
                    return;
                }
                const currentDate = new Date();
                const currentMonth = currentDate.getMonth() + 1; // Months are zero-indexed
                const currentYear = currentDate.getFullYear(); // Get the last four digits of the current year

                const expirationYear = parseInt(value.slice(0, 4), 10);
                const expirationMonth = parseInt(value.slice(5, 7), 10);

                if (
                    expirationYear < currentYear ||
                    (expirationYear === currentYear && expirationMonth < currentMonth)
                ) {
                    paymentErrors.value.expiration = 'Expiration date must be in the future';
                    return;
                }
            }

            clearPaymentError('expiration'); // Clear error if any
        }

        return {
            isLogoAllowed,
            cartItems,
            paymentData,
            paymentErrors,
            statusMessage,
            visible,
            isError,
            isLoading,
            showMessage,
            discountAmount,
            convenienceAmount,
            salesTaxAmount,
            subTotal,
            finalTotal,
            formatPrice,
            errorPaymentClass,
            clearPaymentError,
            captchaText,
            formatExpirationDate,
            generateCaptcha,
            isPlacingOrder,
            billing,
            orderTotal,
            orderId,
            tokenKey,
            ipAddr,
            handlePayment,
            validatePaymentForm,
            generateInvoiceNumber,
            iframeUrl,
            iframeKey,
            gateway,
            authorize,
            quantum,
            declineReasonMessage,
        };
    },
};
</script>

<style scoped>
.custom-title {
    font-size: 14px;
}

tr {
    border-color: transparent;
}

table {
    --bs-table-bg: transparent !important;
}

input,
select,
.form-check-input {
    border: 1px solid #000;
    background-color: transparent;
    border-radius: 0;
}

input:focus,
.form-select:focus {
    outline: none;
    border-radius: 0;
    border: 1px solid #000;
    box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.1);
}

.btn {
    transition: all 0.3s ease;
    text-transform: uppercase;
    letter-spacing: 1px;
}

.btn:hover:not(:disabled) {
    opacity: 0.9;
    transform: translateY(-1px);
}

.btn:active:not(:disabled) {
    transform: translateY(0);
}

.captcha-text {
    background-color: #f8f9fa;
    border: 1px dashed #dee2e6;
}

.no-select {
    user-select: none;
    pointer-events: none;
}

@media (max-width: 320px) {
    .box {
        margin: 0 15px;
    }

    .overlay-text p {
        font-size: x-small !important;
    }

    .no-border-end {
        border-right: none !important;
    }
}

@media (min-width: 321px) and (max-width: 425px) {
    .box {
        margin: 0 15px;
    }

    .overlay-text p {
        font-size: small !important;
    }
    .overlay-text p.is-max {
    font-size: 10px !important;
  }
    .no-border-end {
        border-right: none !important;
    }
}

@media (min-width: 426px) and (max-width: 575px) {
    .box {
        margin: 0 15px;
    }

    .no-border-end {
        border-right: none !important;
    }

    .btn {
        margin-top: 10px;
        width: 100%;
    }

    .overlay-text p {
        font-size: large !important;
    }
    .overlay-text p.is-max {
    font-size: 15px !important;
  }
}

@media (min-width: 576px) and (max-width: 767px) {
    .no-border-end {
        border-right: none !important;
    }

    .box {
        margin: 0 15px;
    }

    .overlay-text p {
        font-size: 6px !important;
    }
}

@media (min-width: 768px) and (max-width: 991px) {
    .no-border-end {
        border-right: none !important;
    }

    .overlay-text p {
        font-size: 10px !important;
    }
    .overlay-text p.is-max {
    font-size: 9px !important;
  }
}

@media (min-width: 992px) and (max-width: 1200px) {
    .overlay-text p {
        font-size: 6px !important;
    }
    .overlay-text p.is-max {
    font-size: 5px !important;
  }
}

@media (min-width: 1201px) and (max-width: 1400px) {
    .overlay-text p {
        font-size: 8px !important;
    }
    .overlay-text p.is-max {
    font-size: 7px !important;
  }
}

@media (min-width: 1401px) {
    .overlay-text p {
        font-size: 9px !important;
    }
    .overlay-text p.is-max {
    font-size: 8px !important;
  }
}


.payment-logos {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 15px;
    margin-top: 20px;
}

.quantum-form .payment-logos {
    margin-top: -70px;
}

.payment-icon {
    width: 60px;
    height: auto;
    object-fit: contain;
}

/* Medium devices (Tablets: 768px and below) */
@media (max-width: 768px) {


    .payment-icon {
        width: 50px;
    }
}

/* Small devices (Mobile: 480px and below) */
@media (max-width: 480px) {


    .payment-icon {
        width: 40px;
    }
}
</style>