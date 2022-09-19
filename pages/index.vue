<template>
	<div>
		<c-tabs 
			isFitted
			variant="line" 
			variant-color="vue"
			mt="1em"
		>
			<c-tab-list color="gray.500">
				<c-tab>
					<c-heading size="sm">RR calculator</c-heading>
				</c-tab>
				<c-tab>
					<c-heading size="sm">Position Sizing</c-heading>
				</c-tab>
				<c-tab>
					<c-heading size="sm">Trade grade</c-heading>
				</c-tab>
			</c-tab-list>

			<c-tab-panels>
				<c-tab-panel>
					<Main />
				</c-tab-panel>
				<c-tab-panel>
					<Position />
				</c-tab-panel>
				<c-tab-panel>
					<c-heading>Trade grade</c-heading>
				</c-tab-panel>
			</c-tab-panels>
		</c-tabs>
	</div>
</template>
