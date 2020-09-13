<template>
    <v-select
        :id="name"
        :value="value"
        :options="choices"
        :placeholder="options.placeholder"
        :icon="options.icon"
        :other="options.allow_other"
        @input="$emit('input', $event)"
    ></v-select>
</template>

<script>
import mixin from "@directus/extension-toolkit/mixins/interface";

export default {
    mixins: [mixin],

    data() {
        return {
            parseError: null
        };
    },

    mounted() {
        // Get list with available fields for the current collection.
        const { values } = this._props;

        this.handleChoice();
    },

    computed: {
        choices() {
            // Available choices for this field.
            // Cloning the object to prevent mutations of the original choise list.
            let choices = Object.assign({}, this.options.choices);

            // Transform the choice value to the key.
            for (const key of Object.keys(choices)) {
                let newValue = key;

                // If formatting is enabled, format the key.
                //     - Capitalize
                //     - Replace "_" by spaces
                if (this.options.formatting) {
                    newValue = newValue.replace(/_/g, " ");
                    newValue = newValue.replace(
                        /(^\w{1})|(\s{1}\w{1})/g,
                        match => match.toUpperCase()
                    );
                }

                choices[key] = newValue;
            }

            return choices;
        }
    },

    methods: {
        /**
         * Handle a specific selected choise of the input.
         * This will show the fields linked to the given choice &
         * will hide all the other fields linked to the other choices.
         */
        handleChoice() {
            // Available choices inside the dropdown.
            const choices = this.options.choices;

            // Selected choice for this field.
            const selected = this.value;

            // Fields for the selected choice.
            const selectedFields = choices[selected];

            // Flattened list with all the other conditional fields that are available in the choices list.
            const otherFields = Object.keys(choices)
                .filter(key => key !== selected)
                .map(key => choices[key])
                .flat();

            // Hide the other fields first before showing the selected fields.
            // This is to make sure that overlapping conditional fields between choices remain visible at the correct time.
            for (const field of otherFields) {
                this.hideField(field);
            }

            // Show the correct conditional fields.
            // Only when the selected value is not undefined.
            if (selected) {
                for (const field of selectedFields) {
                    this.showField(field);
                }
            }
        },

        /**
         * Hide a field with a given name.
         * @param {string} name Name of the field
         */
        hideField(name) {
            const node = this.getFieldNode(name);

            if (node && node.style) {
                node.style.display = "none";
            }
        },

        /**
         * Show a field with a given name.
         * @param {string} name Name of the field
         */
        showField(name) {
            const node = this.getFieldNode(name);

            if (node && node.style) {
                node.style.display = "block";
            }
        },

        /**
         * Get a field with a given name.
         * @param {string} name Name of the field
         * @returns {Node} Field node.
         */
        getFieldNode(name) {
            return document.querySelector(`[data-field="${name}"]`);
        }
    },

    watch: {
        /**
         * Watch the value for changes to the choice.
         * Trigger the "handleChoice" for dynamic changing of fields.
         */
        value() {
            this.handleChoice();
        }
    }
};
</script>

<style lang="scss" scoped>
.v-select {
    margin-top: 0;
    max-width: var(--width-medium);
}
</style>
