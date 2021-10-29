<template>
	<transition :name="transitionName">
		<div class="toast" :class="toastClasses" v-show="show">
			<div class="toast-icon">
				<component :is="toastIcon"></component>
			</div>
			<div class="toast-content">
				<div class="toast-title">{{toastTitle}}</div>
				<div class="toast-message">{{message}}</div>
			</div>
			<button class="toast-button" @click="$emit('hide')">&times;</button>
		</div>
	</transition>
</template>

<script>
	import IconError from './IconError.vue';
	import IconWarning from './IconWarning.vue';
	import IconSuccess from './IconSuccess.vue';

	export default {
		data() {
			return {
				timeout: null
			}
		},
		components: {
			iconError: IconError,
			iconWarning: IconWarning,
			iconSuccess: IconSuccess
		},
		props: {
			message: {
				type: String,
				required: true
			},
			title: {
				type: String,
				default: null
			},
			show: {
				type: Boolean,
				default: false
			},
			type: {
				type: String,
				default: 'success',
				validator: (value) => {
					return ['success', 'warning', 'error'].indexOf(value) !== -1;
				}
			},
			position: {
				type: String,
				default: 'bottom-right'
			}
		},
		emits: ['hide'],
		watch: {
			show() {
				if(this.timeout) {
					clearTimeout(this.timeout);
				}
				this.timeout = setTimeout(() => {
					this.$emit('hide');
				}, 3000);
			}
		},
		computed: {
			toastType() {
				return `toast-${this.getType}`;
			},
			toastIcon() {
				return `icon-${this.getType}`;
			},
			getType() {
				return ['success', 'warning', 'error'].indexOf(this.type) === -1 ? 'success' : this.type;
			},
			toastTitle() {
				return this.title ? this.title : this.type.charAt(0).toUpperCase() + this.type.slice(1);
			},
			getPosition() {
				return ['bottom-left', 'bottom-right', 'top-left', 'top-right'].indexOf(this.position) === -1 ? 'bottom-right' : this.position;
			},
			toastClasses() {
				return [this.toastType, this.position];
			},
			transitionName() {
				const transitions = {
					'top-left': 'rtl',
					'bottom-left': 'rtl',
					'top-right': 'ltr',
					'bottom-right': 'ltr'
				};
				return transitions[this.getPosition];
			}
		},
		
	}
</script>

<style lang="scss" scoped>
	.toast {
		width: 300px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 1rem;
		box-shadow: 1px 5px 10px -5px rgba(0, 0, 0, .2);
		position: relative;

		&:before {
			content: "";
			width: 4px;
			height: 100%;
			
			position: absolute;
			top: 0;
			left: 0;
		}

		.toast-icon {
			width: 16px;
			height: 16px;
			border-radius: 50%;
			padding: 7px;
		}

		&.toast-success {
			background-color: #ecfdf5;

			&:before {
				background-color: #34d399;
			}

			.toast-icon {
				background-color: #34d399;

				svg {
					fill: #ecfdf5;
				}
			}
		}

		&.toast-warning {
			background-color: #fffbeb;

			&:before {
				background-color: #f59e0b;
			}

			.toast-icon {
				background-color: #f59e0b;

				svg {
					fill: #fffbeb;
				}
			}
		}

		&.toast-error {
			background-color: #fef2f2;

			&:before {
				background-color: #ef4444;
			}

			.toast-icon {
				background-color: #ef4444;

				svg {
					fill: #fef2f2;
				}
			}
		}

		&.bottom-left {
			position: fixed;
			left: 20px;
			bottom: 20px;
		}

		&.top-left {
			position: fixed;
			left: 20px;
			top: 20px;
		}

		&.bottom-right {
			position: fixed;
			right: 20px;
			bottom: 20px;
		}

		&.top-right {
			position: fixed;
			right: 20px;
			top: 20px;
		}

		.toast-content {
			flex-grow: 1;
			margin: 0 1rem;

			.toast-title {
				font-weight: 700;
				margin-bottom: .5rem;
			}

			.toast-message {
				font-size: 14px;
				color: #6b7280;
			}
		}

		.toast-button {
			width: 1.5em;
			height: 1.5em;
			border: none;
			padding: 0;
			color: #9ca3af;
			opacity: .7;
			background: transparent;
			cursor: pointer;
			font-size: 1.5em;

			&:hover {
				opacity: 1;
			}
		}
	}

	.rtl-enter-active, .rtl-leave-active,
	.ltr-enter-active, .ltr-leave-active {
		transition: all .5s ease-in-out;
	}

	.ltr-enter-from,.ltr-leave-to {
		transform: translateX(100%);
	}

	.rtl-leave-to,.ltr-leave-to  {
		opacity: 0;
	}

	.rtl-enter-from,.rtl-leave-to {
		transform: translateX(-100%);
	}
</style>