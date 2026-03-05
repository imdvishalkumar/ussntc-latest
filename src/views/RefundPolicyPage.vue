<template>
    <div class="policy-page">
        <div class="container py-5">
            <h1 class="page-title mb-4">Refund Policy – Vortex Legacy Group</h1>
            
            <div class="policy-content">
                <section class="mb-4">
                    <p><strong>Company: Vortex Legacy Group ("Vortex," "we," "us," or "our")</strong></p>
                </section>

                <section class="mb-4">
                    <h2>1. Overview – Custom & Engraved Items</h2>
                    <p>
                        All products sold by Vortex Legacy Group are custom-made and/or engraved to order. Because these items are personalized specifically for each customer, all sales are final, subject only to the limited shipping-damage exception described below.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>2. All Sales Final – No Returns or Refunds</h2>
                    <p>
                        We do not accept returns, refunds, or exchanges for any reason, including buyer’s remorse or ordering errors since all of our products have custom engraving.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>2A. Order Approval, Production, and No Changes</h2>
                    <p>
                        After 24 hours of placing your order, it will be submitted for approval and scheduled for production. Once an order has been approved and scheduled for production, changes, cancellations, and refunds are not possible.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>2B. Custom Engraving – No Refunds After Engraving</h2>
                    <p>
                        Due to custom engraving, once an item has been engraved, it is not refundable under any circumstances.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>3. Customer Responsibility for Personalization</h2>
                    <p>
                        Customers are solely responsible for reviewing and confirming all personalization details submitted at checkout. Errors provided by the customer are not eligible for replacement or refund.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>4. Limited Exception – Items Arriving Broken or Damaged</h2>
                    <p>
                        If an item is received broken or damaged due to shipping, Vortex Legacy Group must be notified within seven (7) days of delivery. Notification must include clear photographic documentation of the damaged item and the original packaging. Claims submitted without photos or after the 7-day period will not be eligible for replacement or refund.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>5. What Is Not Considered Damage</h2>
                    <p>
                        Minor variations, natural material characteristics, wear and tear, damage after delivery, improper handling, installation, or dissatisfaction do not qualify as damage.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>6. Shipping Issues & Delivered Packages</h2>
                    <p>
                        Packages marked as delivered by the carrier are considered delivered. Vortex Legacy Group is not responsible for stolen or missing packages after confirmed delivery.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>7. Limitation of Remedies</h2>
                    <p>
                        To the maximum extent permitted under Idaho law, the exclusive remedy for any valid claim is replacement of the item or, if replacement is not feasible, refund of the purchase price.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>8. Idaho Consumer Protection Compliance</h2>
                    <p>
                        Nothing in this policy waives rights that cannot be waived under the Idaho Consumer Protection Act.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>9. Governing Law & Venue</h2>
                    <p>
                        This policy is governed by the laws of the State of Idaho. Venue shall be in Ada County, Idaho or the United States District Court for the District of Idaho, as applicable.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>10. Contact Information</h2>
                    <p>Email: <a href="mailto:support@vortexlegacygroup.com">support@vortexlegacygroup.com</a></p>
                    <p v-if="contactInfo.phone_number" class="contact-info">
                        <strong>{{ contactInfo.phone_number }}</strong>
                    </p>
                    <p>
                        Or visit our <router-link to="/contact">Contact Page</router-link>.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>11. Policy Updates</h2>
                    <p>
                        This policy may be updated at any time and applies to future purchases only.
                    </p>
                </section>
            </div>
        </div>
    </div>
</template>

<script>
import { computed } from 'vue';
import { useCampaignStore } from '@/store/campaign';

export default {
    name: 'RefundPolicyPage',
    setup() {
        const campaignStore = useCampaignStore();
        const contactInfo = computed(() => campaignStore.contactInfo);

        return {
            contactInfo,
        };
    },
};
</script>

<style scoped>
.policy-page {
    min-height: 100vh;
    background-color: #f8f9fa;
}

.page-title {
    color: #fd5a1e;
    font-weight: bold;
    font-size: 2.5rem;
    margin-top: 13px;
}

.policy-content {
    background: white;
    padding: 2rem;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.policy-content h2 {
    color: #27251f;
    font-size: 1.5rem;
    margin-bottom: 1rem;
    font-weight: 600;
}

.policy-content p {
    color: #333;
    line-height: 1.8;
    margin-bottom: 1rem;
    text-align: left;
}

.policy-content ul {
    margin-left: 1.5rem;
    margin-bottom: 1rem;
    text-align: left;
}

.policy-content li {
    color: #333;
    line-height: 1.8;
    margin-bottom: 0.5rem;
    text-align: left;
}

.policy-content strong {
    color: #27251f;
    font-weight: 600;
}

.policy-content .contact-info {
    font-size: 1.1rem;
    color: #fd5a1e;
    margin-top: 0.5rem;
}

.policy-content a {
    color: #fd5a1e;
    text-decoration: none;
}

.policy-content a:hover {
    text-decoration: underline;
}

@media (max-width: 768px) {
    .page-title {
        font-size: 2rem;
    }

    .policy-content {
        padding: 1.5rem;
    }

    .policy-content h2 {
        font-size: 1.25rem;
    }
}
</style>
