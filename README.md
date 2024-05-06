# weddingimport java.io.ByteArrayOutputStream;
import java.io.FileInputStream;
import java.util.HashMap;
import java.util.Map;

import org.pentaho.reporting.engine.classic.core.ClassicEngineBoot;
import org.pentaho.reporting.engine.classic.core.DefaultReportEnvironment;
import org.pentaho.reporting.engine.classic.core.ReportProcessingException;
import org.pentaho.reporting.engine.classic.core.ResourceBundleFactory;
import org.pentaho.reporting.engine.classic.extensions.modules.ReportModuleExtension;
import org.pentaho.reporting.engine.classic.wizard.DefaultReportWizard;
import org.pentaho.reporting.libraries.repository.ContentItem;
import org.pentaho.reporting.libraries.repository.ContentRepository;

public class PrptReportGenerator {

    public static byte[] generateReport(String prptFilePath, Map<String, Object> parameters) throws ReportProcessingException {
        ClassicEngineBoot.getInstance().init();

        ContentRepository contentRepository = (ContentRepository) ResourceBundleFactory.getInstance().getResource("ContentRepository");
        ContentItem reportContent = contentRepository.getContentItem(prptFilePath);

        DefaultReportWizard reportWizard = new DefaultReportWizard();
        reportWizard.setErrorHandler(new DefaultReportEnvironment());
        reportWizard.readReportDefinition(reportContent);

        if (parameters != null) {
            for (Map.Entry<String, Object> entry : parameters.entrySet()) {
                reportWizard.setParameter(entry.getKey(), entry.getValue());
            }
        }

        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        reportWizard.generateReport(outputStream, null);

        return outputStream.toByteArray();
    }

    public static void main(String[] args) throws Exception {
        // Replace with your actual .prpt file path
        String filePath = "path/to/your/report.prpt";

        // Optional: Set report parameters (key-value pairs)
        Map<String, Object> parameters = new HashMap<>();
        parameters.put("myParameter1", "value1");

        byte[] reportContent = generateReport(filePath, parameters);

        // Write report content to a file (replace with your desired output logic)
        // ...
    }
}
