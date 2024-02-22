<template>
	<MaterialLabel
		:label="label"
		:value="value"
		@click="$emit('click')"
		class="textinput"
		:focus="focus"
		:class="{ 'pointer-events-none opacity-75': disabled }"
	>
		<input
			v-if="type == 'text' || type == 'password' || type == 'date'"
			:type="type === 'password' ? 'password' : 'text'"
			:name="name"
			ref="input_"
			@focus="onfocus"
			@blur="focus = false"
			class="materiallabel-input"
			:class="{
				'pointer-events-none ': type === 'date',
			}"
			:disabled="disabled || type === 'date'"
			:id="type === 'date' ? id : null"
			placeholder=" "
			autocomplete="off"
			readonly
			@input="onChange"
			:value="value"
		/>
		<input
			v-else-if="type === 'number'"
			@focus="focus = true"
			@blur="focus = false"
			type="text"
			:min="min"
			:max="max"
			:name="name"
			class="materiallabel-input"
			:disabled="disabled"
			placeholder=" "
			@input="onChange"
			:value="value"
		/>
		<Select
			v-else-if="type === 'select'"
			:disabled="disabled"
			:optionName="optionName"
			:optionValue="optionValue"
			:value="value"
			:endIcon="endIcon"
			inputClass="materiallabel-input bg-white"
			@change="changeInput"
			:list="options"
			@focus="focus = true"
			@blur="focus = false"
		/>
		<Select2
			v-else-if="type === 'select2'"
			:optionName="optionName"
			:optionValue="optionValue"
			:value="value"
			inputClass="materiallabel-input bg-white"
			@change="changeInput"
			:endIcon="endIcon"
			:list="options"
			@focus="focus = true"
			@blur="focus = false"
		/>
		<div
			v-else-if="endIcon && type !== 'select' && type !== 'select2'"
			class="
				textinput-icon
				absolute
				duration-300
				end-0
				top-0
				bottom-0
				flex
				items-center
				justify-center
			"
			@click="$emit('click-icon')"
		>
			<MyIcon :name="endIcon" class="w-8 h-8 me-5" />
		</div>
		<input v-else-if="type === 'date2'" type="date" :name="name"
			@focus="focus = true"
			@blur="focus = false"
			class="materiallabel-input"
			:disabled="disabled"
			@input="onDateChange"
			:value="value"/>
		<div
			v-if="type === 'date'"
			class="
				w-full
				h-full
				absolute
				left-0
				right-0
				top-0
				bottom-0
				cursor-pointer
			"
			@click="show = true"
		/>
		<textarea
			v-if="type === 'textarea'"
			class="materiallabel-input resize-none outline-none border-0 h-full"
			:value="value"
			placeholder=" "
			@input="onChange"
			:name="name"
			:disabled="disabled"
			@focus="focus = true"
			@blur="focus = false"
		></textarea>
		<client-only>
			<VuePersianDatetimePicker
				v-if="type === 'date'"
				:element="id"
				color="#26CDAB"
				:editable="true"
				@close="show = false"
				:value="value"
				@input="onDateChange"
				format="jYYYY/jMM/jDD"
				:show="show"
			/>
		</client-only>
	</MaterialLabel>
</template>

<script lang="ts">
import Vue, { PropOptions } from 'vue'
import MaterialLabel from '~/components/utils/MaterialLabel.vue'
import Select from '~/components/utils/select/Select.vue'
import Select2 from '~/components/utils/select/Select2.vue'
import MyIcon, { MyIconNameType } from '~/components/utils/MyIcon.vue'

export default Vue.extend({
	components: {
		MyIcon,
		Select,
		Select2,
		VuePersianDatetimePicker: () =>
			process.client
				? import('vue-persian-datetime-picker')
				: Promise.resolve({ MaterialLabel }),
	},
	model: {
		prop: 'value',
		event: 'change',
	},
	data() {
		return {
			id: null,
			show: false,
			focus: false,
		}
	},
	mounted() {
		// @ts-ignore
		this.id = `textinput-${this._uid}`
	},
	props: {
		value: [String, Number],
		name: {
			default: '',
			type: String,
		},
		label: {
			default: '',
			type: String,
		},
		type: {
			default: 'text',
			type: String,
		} as PropOptions<
			| 'text'
			| 'number'
			| 'select'
			| 'password'
			| 'date'
			| 'textarea'
			| 'select2'
		>,
		options: {
			default: null,
			type: Array,
		} as PropOptions<any[]>,
		disabled: {
			type: Boolean,
			default: false,
		},
		endIcon: {
			default: null,
			type: String,
		} as PropOptions<MyIconNameType>,
		optionName: {
			default: null,
			type: [String, Number],
		},
		optionValue: {
			default: null,
			type: [String, Number],
		},
		min: {
			default: 1,
			type: Number,
		},
		max: {
			default: 10,
			type: Number,
		},
	},
	methods: {
		changeInput(v: any) {
			this.$emit('change', v)
		},
		numberChange(e: any) {},
		onDateChange(v: any): void {
			this.$emit('change', v.target.value)
		},
		persianNumberToEnglish(v: string) {
			let r = []
			for (let i = 0; i < v.length; i++)
				if (v.charCodeAt(i) >= 1776 && v.charCodeAt(i) <= 1785)
					r.push(String.fromCharCode(v.charCodeAt(i) - 1728))
				else r.push(v[i])

			return r.join('')
		},
		
		onChange(e: any): void {
			let v = (e.target?.value || '') as string

			if (this.type === 'number') {
				const t = this.persianNumberToEnglish(v)
				if (isNaN(Number(t)) || Number(t) > this.max) {
					e.target.value = ''
					return
				}
				this.$emit('input', e)
				this.$emit('change', t)
				return
			}
			if (this.type !== 'password'){

				let x:string = e.target.value
				x = x.replace(/[<>&\\]/g, '')
				e.target.value = x
			}
			this.$emit('input', e)
			this.$emit('change', e.target.value)
		},
		onfocus(e: any){
			(this.$refs.input_ as any).removeAttribute('readonly')
			this.focus = true
		}
	},
})
</script>

<style lang="postcss" scopped>
.textinput .textinput-icon {
	@apply text-text-primary-dark;
}
.textinput:focus-within .textinput-icon {
	@apply text-primary;
}

.vpd-next,
.vpd-prev {
	@apply flex items-center justify-center;
}
.vpd-next svg,
.vpd-prev svg {
	@apply m-auto;
}
.vpd-content {
	@apply rounded-2xl;
}
.vpd-content .vpd-header {
	@apply rounded-t-2xl;
}
.vpd-content button {
	@apply rounded-xl;
}
.vpd-actions {
	@apply flex justify-evenly;
}
</style>
