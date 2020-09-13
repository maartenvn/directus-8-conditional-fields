<template>
    <span class="no-wrap">{{ displayValue }}</span>
</template>

<script>
import mixin from '@directus/extension-toolkit/mixins/interface';

export default {
	mixins: [mixin],
	computed: {
		displayValue() {
            let value = this.value;

			// If formatting is enabled, format the key.
            //     - Capitalize
            //     - Replace "_" by spaces
            if(this.options.formatting) {
                value = value.replace(/_/g, " ");
                value = value.replace(/(^\w{1})|(\s{1}\w{1})/g, match => match.toUpperCase());
            }

            return value;
		}
	}
};
</script>
