package handler;

import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;
import com.amazonaws.services.lambda.runtime.events.APIGatewayProxyRequestEvent;
import com.amazonaws.services.lambda.runtime.events.APIGatewayProxyResponseEvent;
import layer.service.APIGatewayService;
import layer.service.APIGatewayServiceImpl;
import layer.service.DynamoDBService;
import layer.service.DynamoDBServiceImpl;

public class UpdateUserFunction implements RequestHandler<APIGatewayProxyRequestEvent, APIGatewayProxyResponseEvent> {
    private static final DynamoDBService dynamoDBService = new DynamoDBServiceImpl();
    private static final APIGatewayService apiGatewayService = new APIGatewayServiceImpl();

    public APIGatewayProxyResponseEvent handleRequest(final APIGatewayProxyRequestEvent input,
                                                      final Context context) {
        try {
            // Extract parameters and body from input
            Map<String, String> pathParameters = input.getPathParameters();
            Map<String, String> bodyParameters = apiGatewayService.getBodyParameters(input.getBody());

            // TODO: Implement logic to update user record using DynamoDBService
            // For example:
            String userId = pathParameters.get("userId");
            String updatedData = bodyParameters.get("updatedData");

            // Call DynamoDBService to update the record
            dynamoDBService.updateUser(userId, updatedData);

            return apiGatewayService.getApiGatewayProxyResponseEvent("User updated successfully", 200);
        } catch (Exception e) {
            return apiGatewayService.getApiGatewayProxyResponseEvent(
                    "An error occurred while executing the lambda function: "
                            + e.getClass() + "; message: " + e.getMessage(),
                    503);
        }
    }
}
