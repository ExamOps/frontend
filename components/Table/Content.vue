<script setup lang="ts">
    type Props = {
        data: any[];
        call: any;
        api: any;
        title: string;
        styles?: Record<string, string>;
        isExpanded?: boolean;
    }
    const props = defineProps<Props>();
    const emit = defineEmits(['active']);

    const hoveredRow = shallowRef(null);
    const activeRow = defineModel<string>();
    
    const className = (index: number, data: any[]) => {
        if (index === 0) return 'first';
        if (index === data.length - 1) return 'last';
        return '';
    }

    const borderBottom = (index: number, data: any[]) => {
        if (data.length < 25)
            return index === data.length - 1 ? 'none' : '1px solid var(--secondary-color)';
            
        return index === data.length - 1 ? 'none' : '1px solid var(--secondary-color)';
    };
</script>

<template>
    <TableContainer 
        :count="Object.keys(data[0]).length"
        :countQueries="data.length" 
        :title="title"
        :styles="styles"
    >
        <template #header>
            <TableHeaderColumn 
                v-for="key in Object.keys(data[0])" 
                :key="key"
                :column="key"
            />
        </template>

        <template #content>
            <slot>
                <TableDataRow 
                    v-for="(row, index) in data" 
                    :key="row"
                    :item="row"
                    :call="call"
                    :api="api"
                    v-model="activeRow"
                    @mouseenter="hoveredRow = row"
                    @mouseleave="hoveredRow = null"
                >
                    <template #columns>
                            <TableDataColumn 
                            v-for="(column, columnIndex) in Object.keys(row)" 
                            :key="column" 
                            :hovered="hoveredRow === row"
                            :active="activeRow === row.cup"
                            :class-name="className(columnIndex, Object.keys(row))"
                            :name="column"
                            :field="row[column]" 
                            :is-last-row="index === data.length - 1"
                            :style="{
                                borderBottom: borderBottom(index, data),
                            }"
                        />
                    </template>

                    <template #expanded>
                        <TableExpand v-if="isExpanded && row.cup === activeRow">
                            <slot name="expanded"></slot>
                        </TableExpand>
                    </template>
                </TableDataRow>
            </slot>
        </template>
    </TableContainer>
</template>