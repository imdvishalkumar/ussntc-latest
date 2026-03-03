<template>
    <div class="policy-page">
        <div class="container py-5">
            <h1 class="page-title mb-4">Refund Policy</h1>
            
            <div class="policy-content">
                <section class="mb-4">
                    <h2>Refund & Return Policy</h2>
                    <p>
                        At Bricks R Us, each engraved brick is custom-made according to the information provided by the customer. Because our products are personalized, we cannot offer refunds or accept returns once production has begun, except in cases where the error is ours.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>Order Accuracy</h2>
                    <p>Please review your order carefully before submitting it.</p>
                    <ul>
                        <li>Bricks are engraved exactly as the information is entered.</li>
                        <li>Bricks R Us is not responsible for errors (including spelling, punctuation, capitalization, or design choices) submitted by the customer.</li>
                    </ul>
                </section>

                <section class="mb-4">
                    <h2>Refunds & Replacements</h2>
                    <ul>
                        <li>If we make an engraving error, we will remake the brick at no cost to you.</li>
                        <li>If the error is due to the customer's submission, a remake can be ordered at the regular price.</li>
                        <li>Refunds are not issued once production has started, unless we are unable to fulfill your order.</li>
                    </ul>
                </section>

                <section class="mb-4">
                    <h2>Damaged or Defective Products</h2>
                    <p>
                        If your brick arrives damaged or defective (not related to engraving), please notify us within 7 days of delivery. We will replace the product at no charge.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>Cancellations</h2>
                    <p>
                        Cancellations must be made within 24 hours of placing the order to be eligible for a full refund. After that time, production may have already begun, and refunds will not be available.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>Quick Refund Policy</h2>
                    <ul>
                        <li>All bricks are custom engraved, so we can't issue refunds once production has started. Please double-check spelling, punctuation, and capitalization before submitting your order.</li>
                        <li>If we make an engraving error, we'll replace your brick at no cost.</li>
                        <li>If the error is in the information provided, a new brick can be ordered at the standard price.</li>
                        <li>Cancellations are only accepted within 24 hours of placing the order.</li>
                        <li>If your brick arrives damaged or defective, contact us within 7 days for a free replacement.</li>
                    </ul>
                </section>

                <section class="mb-4">
                    <h2>Contact Us</h2>
                    <p>For any concerns regarding your order, please contact us at:</p>
                    <p v-if="contactInfo.phone_number" class="contact-info">
                        <strong>{{ contactInfo.phone_number }}</strong>
                    </p>
                    <p v-if="contactInfo.email_address">
                        {{ contactInfo.email_address }}
                    </p>
                    <p>
                        Or visit our <router-link to="/contact">Contact Page</router-link>.
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
