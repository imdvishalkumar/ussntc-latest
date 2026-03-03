<template>
    <div class="policy-page">
        <div class="container py-5">
            <h1 class="page-title mb-4">Terms of Use</h1>
            
            <div class="policy-content">
                <section class="mb-4">
                    <p>
                        These Terms of Use ("Terms") govern your access to and use of the website located at <a href="http://www.the1977experience.com" target="_blank">www.the1977experience.com</a> (the "Site"), including all content, features, functionality, products, services, programs, and transactions offered through the Site (collectively, the "Services"), which are owned and operated by Vortex Legacy Group ("Vortex," "we," "us," or "our").
                    </p>
                    <p>
                        By accessing, browsing, registering for an account, or making a purchase through the Site, you acknowledge that you have read, understood, and agree to be bound by these Terms and our Privacy Policy. If you do not agree, do not use the Site.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>1. Eligibility</h2>
                    <p>You must be at least 18 years old (or the age of majority in your jurisdiction) to use the Site and Services.</p>
                </section>

                <section class="mb-4">
                    <h2>2. Changes to These Terms</h2>
                    <p>We may update or modify these Terms at any time. Continued use of the Site constitutes acceptance of the revised Terms.</p>
                </section>

                <section class="mb-4">
                    <h2>3. Use of the Site & Acceptable Conduct</h2>
                    <p>You agree not to violate laws, infringe rights, upload malicious code, attempt unauthorized access, scrape data, or interfere with Site operations.</p>
                </section>

                <section class="mb-4">
                    <h2>4. User Accounts</h2>
                    <p>You are responsible for maintaining account confidentiality and all activity under your account.</p>
                </section>

                <section class="mb-4">
                    <h2>5. Orders, Payments & Transactions</h2>
                    <p>Payments are accepted directly through the Site. Prices and availability may change. Refunds and fulfillment are governed by posted policies and campaign disclosures.</p>
                </section>

                <section class="mb-4">
                    <h2>6. Campaigns, Fundraising & Partner Programs</h2>
                    <p>Campaign terms may vary. Partner names and trademarks remain the property of their owners.</p>
                </section>

                <section class="mb-4">
                    <h2>7. User Submissions & Uploaded Content</h2>
                    <p>You represent that you own or have rights to submitted content and grant Vortex a license to use such content for operating and fulfilling services.</p>
                </section>

                <section class="mb-4">
                    <h2>8. Intellectual Property Rights</h2>
                    <p>All Site content is owned by or licensed to Vortex and protected by law.</p>
                </section>

                <section class="mb-4">
                    <h2>9. Third-Party Links</h2>
                    <p>Vortex is not responsible for third-party sites or services.</p>
                </section>

                <section class="mb-4">
                    <h2>10. Disclaimers</h2>
                    <p>The Site is provided "AS IS" and "AS AVAILABLE" without warranties.</p>
                </section>

                <section class="mb-4">
                    <h2>11. Limitation of Liability</h2>
                    <p>Vortex shall not be liable for indirect or consequential damages. Total liability is limited to the greater of $100 or fees paid in the prior six months.</p>
                </section>

                <section class="mb-4">
                    <h2>12. Indemnification</h2>
                    <p>You agree to indemnify and hold Vortex harmless from claims arising from your use of the Site or violation of these Terms.</p>
                </section>

                <section class="mb-4">
                    <h2>13. Arbitration Agreement</h2>
                    <p>All disputes shall be resolved by binding arbitration in the State of Idaho. Class actions are waived.</p>
                </section>

                <section class="mb-4">
                    <h2>14. Governing Law</h2>
                    <p>These Terms are governed by the laws of the State of Idaho.</p>
                </section>

                <section class="mb-4">
                    <h2>15. Termination</h2>
                    <p>Vortex may suspend or terminate access for violations of these Terms.</p>
                </section>

                <section class="mb-4">
                    <h2>16. Severability & Waiver</h2>
                    <p>If any provision is unenforceable, the remainder shall remain in effect.</p>
                </section>

                <section class="mb-4">
                    <h2>17. Entire Agreement</h2>
                    <p>These Terms constitute the entire agreement between you and Vortex regarding the Site.</p>
                </section>

                <section class="mb-4">
                    <h2>18. Contact Us</h2>
                    <p>If you have any questions or concerns about these Terms, please contact us at:</p>
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
    name: 'TermsPage',
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

.policy-content h3 {
    color: #27251f;
    font-size: 1.2rem;
    margin-bottom: 0.75rem;
    margin-top: 0.5rem;
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
