# E-Commerce Order Processing Flow

## Process Overview

1. **Start**: Customer places order
2. **Validate Order**: Check inventory and payment
3. **Decision Point 1**: Is inventory available?
   - **Yes**: Proceed to payment validation
   - **No**: Notify customer → End
4. **Decision Point 2**: Is payment valid?
   - **Yes**: Process order
   - **No**: Request payment update → Return to payment validation
5. **Process Order**: Prepare shipment
6. **Ship Order**: Send to customer
7. **End**: Order complete

## Additional Details

- Payment validation can retry up to 3 times
- Inventory check queries real-time database
- Shipment preparation includes packaging and label generation
