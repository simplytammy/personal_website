<template>
	<div class="contact">
		<section class="og-hero sm contact">
			<Navbar />
			<div class="og-hero--content og-container">
				<div class="animate__animated animate__fadeInUp">
					<p class="info">Say hello</p>
					<h1 class="title">Contact —</h1>
				</div>
			</div>
		</section>
		<section class="og-section">
			<div class="og-container animate__animated animate__fadeInUpBig">
				<div class="og-section--heading">
					<h1 class="sub-title">Leave a message <span class="period"></span></h1>
					<p class="info">I will reply within <span class="text-is-primary">24hours</span></p>
					<p class="info-text">(All fields are required)</p>
				</div>
				<form class="pt half-width og-form-area" @submit.prevent="sendMessage">
					<div class="row">
						<div class="column">
							<div class="og-form ">
								<label for="fullName">Full Name</label>
								<div class="is-relative">
									<input type="text" id="fullName" class="og-form-control og-form-focus" v-model="fullName" required>
									<span class="focus-border"><i></i></span>
								</div>
							</div>
						</div>
					</div>
					<div class="row">
						<div class="column">
							<!-- Fullname input -->
							<div class="og-form">
								<label for="phoneNumber">Phone number</label>
								<div class="is-relative">
									<input type="text" id="phoneNumber" class="og-form-control og-form-focus" v-model="phoneNumber" required>
									<span class="focus-border"><i></i></span>
								</div>
							</div>
						</div>
						<div class="column">
							<!-- Email input -->
							<div class="og-form">
								<label for="email">Email</label>
								<div class="is-relative">
									<input type="text" id="email" class="og-form-control og-form-focus" v-model="email" required>
									<span class="focus-border"><i></i></span>
								</div>
							</div>
						</div>
					</div>
					<div class="row">
						<div class="column">
							<div class="og-form">
								<label for="message">Message</label>
								<div class="is-relative">
									<textarea id="message" class="og-textarea og-form-control og-form-focus" rows="3" v-model="message" required></textarea>
									<span class="focus-border"><i></i></span>
								</div>
							</div>
						</div>
					</div>
					<div class="row">
						<div class="" style="padding:15px;">
							<div id="balls" v-if="isSending">
								<div id="ball-1" class="circle"></div>
								<div id="ball-2" class="circle"></div>
								<div id="ball-3" class="circle"></div>
							</div>
							<button type="submit" class="og-link--button text-default" v-if="!isSending"> Send Message<span class="ti-envelope"></span>
							</button>
						</div>
					</div>
				</form>
			</div>
		</section>
		<Footer />
	</div>
</template>
<script>
	import Navbar from '@/components/Navbar.vue'
	import Footer from '@/components/Footer.vue'
	import axios from 'axios'
	export default {
		name: 'Contact',
		scrollToTop: true,
		components: {
			Navbar,
			Footer,
		},
		data() {
			return {
				fullName: '',
				email: '',
				phoneNumber: '',
				message: '',
				submitBtn: 'Send Message',
				isSending: false
			};
		},
		mounted() {
			if (process.client) {
				this.$scrollTo('#__nuxt', 0, {
					force: true
				})
			}
			this.$nextTick(() => {
				this.$nuxt.$loading.start()
				setTimeout(() => this.$nuxt.$loading.finish(), 3000)
			})
		},
		head() {
			return {
				title: "Contact  -Ogurinka Benjamin",
				meta: [{
					name: 'description',
					content: 'I design User Interfaces and build Interactive applications that define experiences, products and brands.'
				}, {
					property: 'og:title',
					content: "Contact - Ogurinka Benjamin"
				}, {
					property: 'og:site_name',
					content: 'Ogurinka Benjamin'
				}, {
					property: 'og:image',
					content: 'https://res.cloudinary.com/simplytammy/image/upload/v1623217825/cover/Cover.png'
				}, {
					property: 'og:type',
					content: 'website'
				}, {
					name: 'twitter:card',
					content: 'https://res.cloudinary.com/simplytammy/image/upload/v1623217825/cover/Cover.png'
				}, {
					name: 'twitter:description',
					content: 'I design User Interfaces and build Interactive applications that define experiences, products and brands.'
				}, {
					name: 'twitter:image',
					content: 'https://res.cloudinary.com/simplytammy/image/upload/v1623217825/cover/Cover.png'
				}, {
					name: 'robots',
					content: 'index,follow'
				}]
			}
		},
		methods: {
			sendMessage() {
				this.phoneNumber.replace(/ /g, '')
				if (this.phoneNumber == '') {
					Toast.fire({
						icon: 'error',
						title: `Please fill out all fields!`
					})
				} else {
					this.isSending = true;
					axios.post(`https://tammy-app.herokuapp.com/messages/api/v1/send`, {
						fullName: this.fullName,
						email: this.email,
						phoneNumber: this.phoneNumber,
						message: this.message,
					}).then(res => {
						this.fullName = '';
						this.email = '';
						this.phoneNumber = '';
						this.message = '';
						this.isSending = false;
						Swal.fire({
							icon: 'success',
							title: 'Message Sent',
							text: 'I have received your message! I will be in touch!',
						})
					}).catch(err => {
						this.isSending = false;
						console.log(err);
						Toast.fire({
							icon: 'error',
							title: `An error has occured, please try again`
						})
					})
				}
			}
		}
	}
</script>