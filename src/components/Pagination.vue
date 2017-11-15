<template>
    <nav v-if="shouldShowPagination">
        <ul class="pagination justify-content-center">
            <li class="page-item" :class="{'disabled': previousDisabled}">
                <a class="page-link" @click="previousClicked">
                    <span slot="pagination-previous-content">&lt;</span>
                </a>
            </li>
            <li class="page-item" :class="{'disabled': nextDisabled}">
                <a class="page-link" @click="nextClicked">
                    <span slot="pagination-next-content">&gt;</span>
                </a>
            </li>
        </ul>
    </nav>
</template>

<script>
    import range from 'lodash/range';

    export default {
        props: {
            pagination: {
                type: Object,
                default: () => ({}),
            },
        },

        computed: {
            pages() {
                return this.pagination.totalPages === undefined
                    ? []
                    : range(1, this.pagination.totalPages + 1);
            },

            shouldShowPagination() {
                if (this.pagination.totalPages === undefined) {
                    return false;
                }

                if (this.pagination.count === 0) {
                    return false;
                }

                return this.pagination.totalPages > 1;
            },

            currentPage () {
                return this.pagination.currentPage || 1;
            },

            totalPages () {
                let ret = 0
                if (!!this.pagination && !!this.pagination.totalPages) {
                    ret = this.pagination.totalPages
                }
                return ret
            },

            previousDisabled () {
                return this.currentPage === 1;
            },

            nextDisabled () {
                return this.currentPage === this.totalPages
            }
        },

        methods: {
            isActive(page) {
                const currentPage = this.pagination.currentPage || 1;

                return currentPage === page;
            },

            pageClicked(page) {
                if (this.pagination.currentPage === page) {
                    return;
                }

                this.$emit('pageChange', page);
            },

            previousClicked () {
                const page = this.currentPage - 1
                const previousAllowed = page > 0
                if (previousAllowed) {
                    this.$emit('pageChange', page)
                }
            },

            nextClicked () {
                const page = this.currentPage + 1
                const nextAllowed = page <= this.totalPages
                if (nextAllowed) {
                    this.$emit('pageChange', page)
                }
            }
        },
    };
</script>
