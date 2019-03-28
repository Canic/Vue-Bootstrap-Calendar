<template>
    <transition name="modal">
        <div v-show="show">
            <div class="modal" @click.self="clickBackdrop">
                <div class="modal-dialog" ref="dialog">
                    <div class="modal-content">

                        <div class="modal-header">
                            <button type="button" class="close" aria-label="Close" @click="cancel">
                                <span aria-hidden="true">&times;</span>
                            </button>
                            <h4 class="modal-title">{{event.title}}</h4>
                        </div>

                        <div class="modal-body">
                            <p v-if="event.eventPage"><a :href="event.eventPage">{{event.description}}</a></p>
                            <p v-else>{{event.description}}</p>
                        </div>

                        <div class="modal-footer">
                            <button type="button" class="btn btn-danger" @click="cancel">
                                <span v-if="$i18n">{{ $t('generic.cancel') }}</span>
                                <span v-else>Cancel</span>
                            </button>
                            <button type="button" class="btn btn-warning" @click="deleteEvent" v-if="canDeleteEvent">
                                <span v-if="$i18n">{{ $t('generic.delete_event') }}</span>
                                <span v-else>Delete Event</span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
            <div class="modal-backdrop in"></div>
        </div>
    </transition>
</template>

<script>
    import {EVENT_DELETED, CANCEL_DETAILS_EVENT_MODAL, SHOW_DETAILS_EVENT_MODAL} from '../actions';

    export default {
       props: {
           show: {
               type: Boolean,
               default: false
           },
           event: {
               type: Object,
               required: true,
           },
           canDeleteEvent: {
               type: Boolean,
           }
       },

       created () {
           if (this.show)
               document.body.classList.add('modal-open');
            else
               document.body.classList.remove('moda-open');
       },

       beforeDestroy () {
           document.body.classList.remove('modal-open');
       },

       watch: {
           show (value) {
               if (value) {
                   if (!document.body.classList.contains('modal-open')) {
                       document.body.classList.add('modal-open');
                   }
               }
           }
       },

       methods: {
            deleteEvent () {
                this.$emit(EVENT_DELETED, this.event);
                this.closeModal();
            },

            cancel () {
                this.closeModal();
            },

            clickBackdrop () {
                this.cancel();
            },

            closeModal() {
                document.body.classList.remove('modal-open');
                this.$emit(SHOW_DETAILS_EVENT_MODAL, false)
            }
       }
    };
</script>

<style scoped>
    .modal {
        display: block;
    }
    .modal-enter-active, .modal-leave-active {
        transition: opacity .2s
    }
    .modal-enter, .modal-leave-to {
        opacity: 0
    }
    .modal-backdrop {
        opacity: 0.3;
    }
</style>