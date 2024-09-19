# AWSDesafio
# DESAFIO DE ENTREGA DE DELIVERY EM AWS STEPFUNCTIONS
{
  "Comment": "A Step Function for a delivery",
  "StartAt": "Receive Order",
  "States": {
    "Receive Order": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:receiveOrder",
      "Next": "Gerador de Instruções de Delivery"
    },
    "Gerador de Instruções de Delivery": {
      "Type": "Task",
      "Resource": "arn:aws:states:::bedrock:invokeModel",
      "Parameters": {
        "ModelId": "cohere.command-text-v14",
        "Body": {
          "prompt": "Entregue o Pedido com pasciencia, espere cerca de 5 minutos ao chegar ao local, e avise ao seu cliente que reralizou o pedido, em seguida você entrega caso a pessoa enteja em frente ao local de entrega, se não, o volta com o pedido."
        }
      },
      "Next": "Assign Driver"
    },
    "Assign Driver": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:assignDriver",
      "Next": "Track Delivery"
    },
    "Track Delivery": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:trackDelivery",
      "Next": "Envia Notificação"
    },
    "Envia Notificação": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:sendNotification",
      "End": true
    }
  }
}
