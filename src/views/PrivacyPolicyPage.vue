<template>
    <div class="policy-page">
        <div class="container py-5">
            <h1 class="page-title mb-4">Privacy Policy</h1>
            
            <div class="policy-content">
                <section class="mb-4">
                    <p>
                        Vortex Legacy Group ("Vortex Legacy Group," "we," "us," or "our") respects your privacy and is committed to protecting it through this Privacy Policy. This Policy explains how we collect, use, disclose, and safeguard information when you visit our website, interact with our content, or purchase products or services from us (collectively, the "Services").
                    </p>
                    <p>
                        By using our Services, you agree to the practices described in this Privacy Policy.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>1. Information We Collect</h2>
                    <p>We collect information in the following ways:</p>
                    
                    <div class="mb-3">
                        <p><strong>A. Information You Provide to Us</strong></p>
                        <p>When you use our Services, you may provide personal information including your name, email address, phone number, billing and shipping address, order details, communications with us, and marketing preferences.</p>
                    </div>

                    <div class="mb-3">
                        <p><strong>B. Information Collected Automatically</strong></p>
                        <p>When you visit our website, we may automatically collect information such as your IP address, browser type, device identifiers, pages viewed, referring URLs, and usage data through cookies or similar technologies.</p>
                    </div>

                    <div class="mb-3">
                        <p><strong>C. Information from Third Parties</strong></p>
                        <p>We may receive information from third parties such as payment processors, ecommerce platforms, advertising partners, analytics providers, and social media platforms, subject to their respective privacy policies.</p>
                    </div>
                </section>

                <section class="mb-4">
                    <h2>2. How We Use Your Information</h2>
                    <p>We use your information to:</p>
                    <ul>
                        <li>Process and fulfill orders</li>
                        <li>Communicate with you regarding purchases, inquiries, or support</li>
                        <li>Personalize and improve our Services</li>
                        <li>Send marketing communications if you opt in</li>
                        <li>Prevent fraud and enhance security</li>
                        <li>Comply with legal and regulatory obligations</li>
                    </ul>
                </section>

                <section class="mb-4">
                    <h2>3. Cookies and Tracking Technologies</h2>
                    <p>
                        We use cookies, pixels, and similar technologies to operate our website, understand user behavior, improve performance, and measure marketing effectiveness. You may control cookies through your browser settings; however, disabling cookies may impact site functionality.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>4. How We Share Your Information</h2>
                    <p>
                        We may share your information with trusted service providers who assist us with website hosting, payment processing, order fulfillment, marketing, analytics, and customer support. We may also disclose information in connection with a business transaction or if required by law. We do not sell personal information for monetary value.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>5. Payment Processing</h2>
                    <p>
                        Payments are processed securely through third-party payment processors. Vortex Legacy Group does not store full credit card numbers or payment security codes.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>6. Email and SMS Marketing</h2>
                    <p>
                        If you opt in, we may send promotional emails or text messages. You may unsubscribe from emails using the link provided or opt out of SMS messages by replying STOP. Transactional communications related to orders may still be sent.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>7. Data Retention</h2>
                    <p>
                        We retain personal information only as long as necessary to provide our Services, meet legal obligations, resolve disputes, and enforce our agreements.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>8. Data Security</h2>
                    <p>
                        We implement reasonable administrative, technical, and physical safeguards to protect your personal information. However, no data transmission or storage system can be guaranteed to be 100% secure.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>9. Your Privacy Rights</h2>
                    <p>
                        Depending on your jurisdiction, you may have rights to access, correct, or request deletion of your personal information, and to opt out of certain processing activities. Requests may be submitted using the contact information below.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>10. Children’s Privacy</h2>
                    <p>
                        Our Services are not directed to children under the age of 13, and we do not knowingly collect personal information from children.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>11. International Visitors</h2>
                    <p>
                        If you access our Services from outside the United States, your information may be transferred to and processed in the United States or other jurisdictions where our service providers operate.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>12. Changes to This Privacy Policy</h2>
                    <p>
                        We may update this Privacy Policy from time to time. The effective date will be updated accordingly. Continued use of the Services constitutes acceptance of any changes.
                    </p>
                </section>

                <section class="mb-4">
                    <h2>13. Contact Us</h2>
                    <p>If you have any questions or concerns about this Privacy Policy, please contact us at:</p>
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
    name: 'PrivacyPolicyPage',
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

.policy-content .lead-text {
    font-size: 1.2rem;
    margin-bottom: 1.5rem;
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
