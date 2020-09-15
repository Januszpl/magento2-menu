<template>
    <div class="admin__field field field-title">
        <label class="label admin__field-label">
            {{ label }}
        </label>
        <div class="admin__field-control control">
            <!-- TODO: re-add changes from #84 -->
            <v-select
                v-model="selected"
                :options="options"
                :placeholder="placeholder"
                :clearable="false"
            >
                <template v-slot:option="option">
                    {{ option.label }}

                    <template v-if="option.store.length > 0">
                        <span class="vs__dropdown-option__details">
                            {{ option.store.join(', ') }}
                        </span>
                    </template>
                </template>
            </v-select>
        </div>
    </div>
</template>

<script>
    define(['Vue'], function(Vue) {
        Vue.component('auto-complete', {
            props: {
                label: {
                    type: String,
                    required: true
                },
                description: {
                    type: String,
                    required: true
                },
                options: {
                    type: Array,
                    required: true
                },
                item: {
                    type: Object,
                    required: true
                },
                config: {
                    type: Object,
                    required: true
                },
                itemKey: {
                    type: String,
                    default: 'content'
                },
                defaultOptionValue: {
                    type: String,
                    default: 'default'
                }
            },
            data () {
              return {
                  defaultSelectedOption: false
              }
            },
            computed: {
                selected: {
                    get() {
                        var selectedOption = '',
                            optionValue;

                        for (var i = 0; i < this.options.length; i++) {
                            optionValue = this.options[i].value.toString();
                            if (optionValue === this.item[this.itemKey]) {
                                selectedOption = this.options[i];
                            }
                            if (optionValue === this.defaultOptionValue) {
                                this.defaultSelectedOption = this.options[i];
                            }
                        }

                        if (!selectedOption) {
                            selectedOption = this.defaultSelectedOption;
                        }

                        return selectedOption;
                    },
                    set(option) {
                        if (option && typeof option === 'object') {
                            this.item[this.itemKey] = option.value.toString();
                        }
                        else {
                          this.item[this.itemKey] = this.defaultSelectedOption ? this.defaultSelectedOption.value.toString() : '';
                        }
                    }
                },
                placeHolder: function() {
                    return this.config.translation.pleaseSelect + ' ' + this.label.toLocaleLowerCase();
                },
            },
            template: template
        });
    });
</script>
